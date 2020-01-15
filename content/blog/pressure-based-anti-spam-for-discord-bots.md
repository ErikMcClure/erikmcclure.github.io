+++
date = ""
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

The bot has two distinct modes of operation. Under normal operation, when a new member joins the server, they are automatically given `@Member` (and `@New` if it's been enabled) and are immediately allowed to speak. If they trigger the anti-spam, or a moderator uses the `!silence` command, they will have the `@Silence` role added and they'll be restricted to speaking in `#Silence Containment`. By default, silences that happen automatically are never rescinded until a moderator manually unsilences someone. However, some server admins are lazy, so an automatic expiration can be added. A manual silence can also be configured to expire after a set period of time.

It's important to note that when silenced, users have **both** the `@Member` and the `@Silence` role. The reason is because many servers have opt-in channels that are only accessible if you add a role to yourself. Simply removing `@Member` would not suffice to keep them from talking in these channels, but `@Silence` can override these permissions and ensure they can't speak in any channel without having to mess up anyone's assigned roles.

The bot detects a raid if `N` joins happen within `M` seconds. It's usually configured to be something like `3` joins in a `90` second interval for an active server. When this happens, the server enters **raid mode**, which automatically removes `@Member` from all the users that just triggered the raid alert. It sends a message pinging the moderators in `#Moderator Channel` and stops adding `@Member` to anyone who joins while it is still in raid mode. It also changes the server verification level, which actually does have an effect for the newly joined members, because they haven't had any roles assigned to them yet. Moderators can either cancel the raid alert, which automatically adds `@Member` to everyone who was detected as part of the raid alert, or they can individually add `@Member` to people they have vetted. The raid mode automatically ends after `M*2` seconds, which would be `180` in this example.

This method of dealing with raids ensures that the bot cannot be overwhelmed by the sheer number of joins. If it crashes or is taken down, the server stays in raid mode until the bot can recover, and the potential raiders will not be allowed in.

## Pressure System

All the anti-spam logic in the bot is done by triggering an automatic silence, which gives someone the `@Silence` role. The bot determines when to silence someone by analyzing all the messages being sent using a pressure system. In essence, the pressure system is analogous to the gravity function used for calculating the "hotness" of a given post on a site like [Hacker News](https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d). 

Each message a user sends is assigned a "pressure score". This is calculated from the message length, contents, whether it matches a filter, how many newlines it has, etc. This "pressure" is designed to simulate how _disruptive_ the message is to the current chat. A wall of text is extremely disruptive, whereas saying "hi" probably isn't. Attaching 3 images to your message is very disruptive, but embedding a single link isn't that bad. Sending a blank message with 2000 newlines is _incredibly_ disruptive, whereas sending a message with two paragraphs is probably fine. In addition, all messages are assigned a _base pressure_, the minimum amount of pressure that any message will have, even if it's only a single letter.

My bot looks at the following values:

Once we've calculated how disruptive a given message is, we can add it to the user's total pressure score, which is a measure of how disruptive a user is currently being. However, we need to recognize that sending a wall of text everyone 10 minutes probably isn't disruptive, but sending 10 short messages saying "ROFLCOPTER" in 10 seconds is spamming. 