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

The bot detects a raid if `N` joins happen within `M` seconds. It's usually configured to be something like `3` joins in a `90` second interval.