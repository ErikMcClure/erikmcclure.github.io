+++
date = "2020-01-15T19:00:00Z"
draft = true
title = "Pressure Based Anti-Spam for Discord Bots"

+++
Back when Discord was a wee little chat platform with no rate limiting whatsoever, it's API had already been reverse-engineered by a bunch of bot developers who then went around spamming random servers with so many messages it would crash the client. My friends and I were determined to keep our discord server public, so I went about creating the first anti-spam bot for Discord.

I am no longer maintaining this bot beyond simple bugfixes because I have better things to do with my time, and I'm tired of people trying to use it because it has the "best anti-spam" features even after I deprecated it. This post is an effort to educate all the other anti-spam bots on how to ascend beyond a simple "mute someone if they send more than N messages in M seconds" filter and hopefully make better anti-spam bots so I don't have to.

## Architecture

There are disagreements over exactly how an anti-spam bot should work, with a wide-range of preferred behaviors that differ between servers, depending on the community, size of the server, rate of growth of the server, and the administrators personal preferences. Many anti-spam solutions lock down the server by forcing new members to go through an airlock channel, but I consider this overkill and the result of poor anti-spam bots that are bad at actually stopping trolls or responding to raids.

My moderation architecture goes like this:  
{{<pre>}}-- Channels --  
\#Moderation Channel  
\#Raid Containment  
\#Silence Containment  
\#Log Channel

\-- Roles --  
@Member  
@Silence  
@New{{</pre>}}

When first-time setup is run on a server, the bot queries and stores all the permissions given to the `@everyone` role. It adds all these permissions to the `@Member` role, then goes through all existing users and adds the `@Member` role to them. It then **removes all permissions from `@everyone`**, but adds an override for `#Raid Containment` so that anyone without the `@Member` role can only speak in `#Raid Containment`.

Then, it creates the `@Silence` role by adding permission overrides to every single channel on the server that prevent you from _sending_ messages on that channel, except for the `#Silence Containment` channel. This ensures that, no matter what new roles might be added in the future, `@Silence` will always prevent you from sending messages on any channel other than `#Silence Containment`. The admins of the server can configure the visibility of the containment channels. Most servers make it so `#Raid Containment` is only visible to anyone without the `@Member` role, ensure `#Silence Containment` is only visible to anyone with the `@Silence` role, and hide `#Log Channel` from everyone except admins.

This architecture was chosen to allow the bot to survive a mega-raid, so that even if 500+ bot accounts storm the server all at once, if the bot goes down or is rate-limited, they can't do anything because they haven't been given the `@Member` role, and so the server is protected by default. This is essentially an automated airlock that only engages if the bot detects a raid, instead of forcing all new members to go through the airlock. Some admins do not like this approach because they prefer to personally audit every new member, but this is not viable for larger servers.

The `@Member` role being added can also short-circuit Discord's own verification rules, which is an unfortunate consequence, but in practice it usually isn't a problem. However, to help compensate for this, the bot can also be configured to add a temporary `@New` role to newer users that expires after a configurable amount of time. What this role actually does is up to the administrators - usually it simply disables sharing images or embeds.

## Operation

The bot has two distinct modes of operation. Under normal operation, when a new member joins the server, they are automatically given `@Member` (and `@New` if it's been enabled) and are immediately allowed to speak. If they trigger the anti-spam, or a moderator uses the `!silence` command, they will have the `@Silence` role added, any messages sent in the last 5 seconds (by default) will be deleted, and they'll be restricted to speaking in `#Silence Containment`. By default, silences that happen automatically are never rescinded until a moderator manually unsilences someone. However, some server admins are lazy, so an automatic expiration can be added. A manual silence can also be configured to expire after a set period of time.

It's important to note that when silenced, users have **both** the `@Member` and the `@Silence` role. The reason is because many servers have opt-in channels that are only accessible if you add a role to yourself. Simply removing `@Member` would not suffice to keep them from talking in these channels, but `@Silence` can override these permissions and ensure they can't speak in any channel without having to mess up anyone's assigned roles.

The bot detects a raid if `N` joins happen within `M` seconds. It's usually configured to be something like `3` joins in a `90` second interval for an active server. When this happens, the server enters **raid mode**, which automatically removes `@Member` from all the users that just triggered the raid alert. It sends a message pinging the moderators in `#Moderator Channel` and stops adding `@Member` to anyone who joins while it is still in raid mode. It also changes the server verification level, which actually does have an effect for the newly joined members, because they haven't had any roles assigned to them yet. Moderators can either cancel the raid alert, which automatically adds `@Member` to everyone who was detected as part of the raid alert, or they can individually add `@Member` to people they have vetted. The raid mode automatically ends after `M*2` seconds, which would be `180` in this example.

This method of dealing with raids ensures that the bot cannot be overwhelmed by the sheer number of joins. If it crashes or is taken down, the server stays in raid mode until the bot can recover, and the potential raiders will not be allowed in.

## Pressure System

All the anti-spam logic in the bot is done by triggering an automatic silence, which gives someone the `@Silence` role. The bot determines when to silence someone by analyzing all the messages being sent using a pressure system. In essence, the pressure system is analogous to the gravity function used for calculating the "hotness" of a given post on a site like [Hacker News](https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d). 

Each message a user sends is assigned a "pressure score". This is calculated from the message length, contents, whether it matches a filter, how many newlines it has, etc. This "pressure" is designed to simulate how _disruptive_ the message is to the current chat. A wall of text is extremely disruptive, whereas saying "hi" probably isn't. Attaching 3 images to your message is very disruptive, but embedding a single link isn't that bad. Sending a blank message with 2000 newlines is _incredibly_ disruptive, whereas sending a message with two paragraphs is probably fine. In addition, all messages are assigned a _base pressure_, the minimum amount of pressure that any message will have, even if it's only a single letter.

My bot looks at the following values:

* **Max Pressure**: The default maximum pressure is 60.
* **Base Pressure**: All messages generate 10 pressure by default, so at most 6 messages can be sent at once.
* **Embed Pressure**: Each image, link, or attachment in a message generates 8.3 additional pressure, which limits users to 5 images or links in a single message.
* **Length Pressure**: Each individual character in a message generates 0.00625 additional pressure, which limits you to sending 2 maximum length messages at once.
* **Line Pressure**: Each newline in a message generates 0.714 additional pressure, which limits you to 70 newlines in a single message.
* **Ping Pressure**: Every single _unique_ ping in a message generates 2.5 additional pressure, which limits users to 20 pings in a single message.
* **Repeat Pressure**: If the message being sent is the _exact_ same as the previous message sent by this user (copy+pasted), it generates 10 additional pressure, effectively doubling the base pressure cost for the message. This means a user copy and pasting the same message more than twice in rapid succession will be silenced.
* **Filter Pressure**: Any filter set by the admins can add an arbitrary amount of additional pressure if a message triggers that filter regex. The amount is user-configurable.

And here a simplified version of my implementation. Note that I am adding the pressure piecewise, so that I can alert the moderators and tell them exactly which pressure trigger broke the limit, which helps moderators tell if someone just posted too many pictures at once, or was spamming the same message over and over:
{{<pre>}}if w.AddPressure(info, m, track, info.Config.Spam.BasePressure) {
	return true
}
if w.AddPressure(info, m, track, info.Config.Spam.EmbedPressure*float32(len(m.Attachments))) {
	return true
}
if w.AddPressure(info, m, track, info.Config.Spam.EmbedPressure*float32(len(m.Embeds))) {
	return true
}
if w.AddPressure(info, m, track, info.Config.Spam.LengthPressure*float32(len(m.Content))) {
	return true
}
if w.AddPressure(info, m, track, info.Config.Spam.LinePressure*float32(strings.Count(m.Content, "\n"))) {
	return true
}
if w.AddPressure(info, m, track, info.Config.Spam.PingPressure*float32(len(m.Mentions))) {
	return true
}
if len(m.Content) > 0 &amp;&amp; m.Content == track.lastcache {
	if w.AddPressure(info, m, track, info.Config.Spam.RepeatPressure) {
		return true
	}
}{{</pre>}}

Once we've calculated how disruptive a given message is, we can add it to the user's total pressure score, which is a measure of how disruptive a user is currently being. However, we need to recognize that sending a wall of text every 10 minutes probably isn't an issue, but sending 10 short messages saying "ROFLCOPTER" in 10 seconds is definitely spamming. So, before we add the message pressure to the user's pressure, we check how long it's been since that user last sent a message, and _decrease their pressure accordingly_. If it's only been a second, the pressure shouldn't decrease very much, but if it's been 3 minutes or so, any pressure they used to have should probably be gone by now. Most heat algorithms do this non-linearly, but my experiments showed that a linear drop-off tends to produce better results (and is simpler to implement).

My bot implements this by simply decreasing the amount of pressure a user has by a set amount for every `N` seconds. So if it's been 2 seconds, they will lose `4` pressure, until it reaches zero. Here is the implementation for my bot, which is done before any pressure is added:  
{{<pre>}}
timestamp := bot.GetTimestamp(m)
track := w.TrackUser(author, timestamp)
last := track.lastmessage
track.lastmessage = timestamp.Unix()*1000 + int64(timestamp.Nanosecond()/1000000)
if track.lastmessage &lt; last { // This can happen because discord has a bad habit of re-sending timestamps if anything so much as touches a message
	track.lastmessage = last
	return false // An invalid timestamp is never spam
}
interval := track.lastmessage - last

track.pressure -= info.Config.Spam.BasePressure * (float32(interval) / (info.Config.Spam.PressureDecay * 1000.0))
if track.pressure &lt; 0 {
	track.pressure = 0
}
{{</pre>}}

In essence, this entire system is a superset of the more simplistic "`N` messages in `M` seconds". If you only use base pressure and maximum pressure, then these determine the absolute upper limit of how many messages can be send in a given time period, regardless of their content. You then tweak the rest of the pressure values to more quickly catch obvious instances of spamming. The maximum pressure can be altered on a per-channel basis, which allows meme channels to spam messages without triggering anti-spam. Here's what a user's pressure value looks like over time:

{{<img src="https://cdn.discordapp.com/attachments/308275612265480193/479858371579609093/unknown.png" alt="Pressure Graph" width="480">}}

Because this whole pressure system is integrated into the Regex filtering module, servers can essentially create their own bad-word filters by assigning a huge pressure value to a certain match, which instantly creates a silence. These regexes can be used to look for other things that the bot doesn't currently track, like all-caps messages, or for links to specific websites. The pressure system allows integrating all of these checks in a unified way that represents the total "disruptiveness" of an individual user's behavior.

## Worst Case Scenario

A special mention should go to the uncommon but possible worst-case scenario for spamming. This happens when a dedicated attacker really, really wants to fuck up your server for some reason. They can do this by creating 1000+ fake accounts with randomized names, and wait until they've all been on discord for long enough that the "new to discord" message doesn't show up. Then, they have the bots join the server _extremely slowly_, at the pace of maybe 1 per hour. Then they wait another week or so, before they unleash a spam attack that has each account send exactly 1 message, followed by a different account sending another message.

If they do this fast enough, you can detect it simply by the sheer volume of messages being sent in the channel, and automatically put the channel in slow mode. However, in principle, it is completely impossible to automatically deal with this attack. Banning everyone who happens to be sending messages during the spam event will ban innocent bystanders, and if the individual accounts aren't sending messages that fast (maybe one per second), this is indistinguishable from a normal conversation.

My bot has an emergency method for dealing with this where it tracks when a given user sent their first message in the server. You can then use a command to ban everyone who sent their first message in the past `N` seconds. This is very effective when it works, but it is trivial for spammers to get past once they realize what's going on - all they need to do is have their fake accounts say "hi" once while they're trickling in. Of course, a bunch of accounts all saying "hi" and then nothing else may raise enough suspicion to catch the attack before it happens.

## Conclusion

Hopefully this article demonstrates how to make a reliable, extensible anti-spam bot that can deal with pretty much any imaginable spam attack. The code for my deprecated spam bot is [available here](https://github.com/ErikMcClure/sweetiebot) for reference, but the code is terrible and most of it was a bad idea. You cannot add the bot itself to a server anymore, and the support channel is no longer accessible.

My hope is that someone can take these guidelines and make a much more effective, generalized anti-spam bot so I don't have to. I hate web development and i'd rather spend my time building a native webassembly compiler instead of worrying about [weird intermittent cloudflare errors](https://github.com/bwmarrin/discordgo/issues/659) that temporarily break everything, or [badly designed lock systems](https://github.com/bwmarrin/discordgo/issues/513) that deadlock under rare edge cases.