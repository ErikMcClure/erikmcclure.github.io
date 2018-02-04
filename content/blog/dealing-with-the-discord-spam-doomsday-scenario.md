+++
blogimport = true
categories = ["blog"]
date = "2017-08-20T23:29:00Z"
draft = true
title = "Dealing With The Discord Spam Doomsday Scenario"
updated = "2017-08-20T23:29:54.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Maintaining a discord anti-spam bot is difficult due to widespread abuse that public discord servers are subjected to. For large public servers that make high-value targets, or simply servers that are part of an unpopular fandom, the situation is exponentially worse. One server my anti-spam bot protects is subjected to about 1 raid per day. The vast majority of these raids can be handled by the moderators, or they simply spam themselves into being auto-banned by the bot. A few times, however, we got large, coordinated raids on the servers. The worst one was an attempted bot raid with about 40 bots all joining at the same time, some of which then immediately started spamming.

While the bot has mostly been able to fend off the larger raids, as a software engineer I naturally hypothesized about what the most devastating hostile attack against the server could be. This results in 3 primary "Doomsday" scenarios involving an army of 1000+ bots that all join at the same time:

**1) All the bots join immediately and start spamming**
This is the most obvious attack pattern, and is also the easiest to detect and defend against, because 1000+ people just joined the server. There are some more subtle issues that occur when trying to deal with an attack of this magnitude, however.


