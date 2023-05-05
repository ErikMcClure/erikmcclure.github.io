+++
categories = ["blog"]
date = "2023-05-05T13:02:00Z"
title = "Discord Should Remove Usernames Entirely"
[author]
name = "Erik McClure"

+++

[Discord's Recent Announcement](https://discord.com/blog/usernames) made a lot of people mad, mostly because of [Hyrum's Law](https://www.hyrumslaw.com/) - users were relying on unintended observable behavior in the original username system, and are mad that their use-cases are being broken despite very good evidence that the current system is problematic. I think the major issue here is that Discord didn't go far enough, and as a result, it's confusing users who are unaware of the technical and practical reasons for the username change, or what a username is even for.

[There are several issues](https://twitter.com/TeqieLemon/status/1654014041373790208) being brought up with the username change. One is that users are very upset about usernames being ascii-only alphanumeric, presumably because they do not realize that Discord is only ever going to show their usernames for the purposes of adding friends. Their *Display Name* is what everyone will normally see, which can be any arbitrary unicode. Discord only spent a single sentence mentioning the problem with someone's username being written in 𝕨𝕚𝕕𝕖 𝕥𝕖𝕩𝕥 and I think a lot of users missed just how big of a problem this is. Any kind of strange character in a username would be liable to render it completely unsearchable, could easily get corrupted when sent over ascii-only text mediums, and essentially had to be copy+pasted verbatim or it wouldn't work.

However, some users *wanted* to be unsearchable, because they had stalkers or were very popular and didn't want random people finding their discord account. Discriminators and case-sensitivity essentially created a searchability problem which users were utilizing on purpose to make it harder for people to search them. The solution to this is extremely simple, and was in fact a feature of many early chat apps: let the user turn off the ability for people to search for their username. That's what people *actually want*.

What discord is trying to do, and communicating incredibly poorly, is **transform usernames into friend codes**. They say this in a very roundabout way for some reason, and they are also allowing people to essentially *reserve custom friend codes*. This is silly. Discord should instead **replace usernames with friend codes**, and provide an opt-in fuzzy search mechanism that tries to find someone based on their Display Name, if users want to be discoverable that way. Discord should let you either regenerate or completely disable your own friend code, if users don't want random people trying to friend them.

What makes this so silly is that nothing is preventing discord from doing this, because *you log in with your e-mail anyway!* By replacing usernames with display names, Discord has removed all functionality from them aside from friend codes, so they should just turn usernames into friend codes and stop confusing everyone so much. There is absolutely no reason a user should have to keep track of their username, display name, and server specific nicknames, and letting users reserve custom friend codes is never going to work, because everyone is going to fight over common friend codes. Force the friend codes to be random 10-digit alphanumeric strings. Stop pretending they should be anything else. Stop letting people reserve specific ones. 

There is *one exception* to this that I would tolerate: a custom profile URL. If you wanted to allow people with nitro to, for whatever reason, pay to have a special URL that linked to their profile, this could be done on a first-come first-serve basis, and it would be pretty obvious to everyone why it had to be unique and an ascii-compatible URL.

I'm really tired of companies making a decision for good engineering reasons, and then implementing that decision in the most confusing way possible and blaming anyone who complains as luddites who hate change. There are better ways to communicate these kinds of changes. If your users are confused and angry about it, then it's *your fault*, not theirs.