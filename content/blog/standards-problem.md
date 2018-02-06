+++
blogimport = true
categories = ["blog"]
comments = [5011332425221492280, 8528568975955881869, 7372419905066142632, 8673496274665161680, 3340116895267339414, 5694020930163792211, 5872803841584221599]
date = "2012-05-07T01:12:00Z"
title = "The Standards Problem"
updated = "2012-05-07T01:33:57.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
When people [leave the software industry](http://www.scottporad.com/2012/05/06/why-do-web-sites-and-software-take-so-long-to-build-and-why-is-it-so-hard/) citing all the horrors of programming, it confuses me when they blame software development itself as the cause of the problems. Programming is very similar to architecture - both an art and a science. The comparison, however, falls apart when you complain about architecture and buildings having all these well-established best practices. The art of making buildings hasn't really changed all that much in the past 50 years. Programming didn't exist 50 years ago. 

The fact is, we've been building buildings for *thousands of years*. We've been writing programs for around *40*, and in that period we have gone from computers the size of rooms to computers the size of watches. The instant we establish any sort of good practice, it's bulldozed by new technology. Many modern functional programming languages had to be updated to elegantly handle concurrency *at all*, and we've only just barely established the concept of worker threads for efficiently utilizing 4-8 cores as we step into the realm of CPU/GPU collision and the advent of massively parallel processing on a large scale, which once again renders standard concepts of programming obsolete. The fact that software runs at all is, quite simply, astonishing. Windows still has old DOS code dating back to the 1980s having repercussions in Windows 7 (You can't name a folder "con" because it was a [reserved device name](http://blogs.msdn.com/b/oldnewthing/archive/2003/10/22/55388.aspx) in DOS). If they were to completely rewrite the operating system now, it'll be completely obsolete in 20 years anyway and we'd be complaining about NT's lingering 32-bit compatibility issues and old functions not being threadsafe on a 128 core processor and problems *I can't even predict*.

Standards can't exist if we can't agree on the right way to do things, and in software development, we can't agree on the right way to do things because *nobody knows* (and if someone tells you they know, *they're lying*). Just as we all begin to start settling down on good practices, new technology changes the rules again. This further complicates things because we often forget exactly which parts of the technology are changing. There's a reason you write a kernel in C and not F#. Our processor architecture is the same fundamental concept it was almost 30 years ago. Ideally we would have moved away from complex instruction sets now that nobody uses them anymore, but we haven't even done that. Language fanboys forget this and insist that everything should be written in whatever their favorite language is, without having any idea what they're talking about. This results in a giant mess, with literally everyone telling everyone else they're doing it wrong.

That's the software industry today. It's where everyone says everyone else is wrong, because we have no goddamn idea what software development should be, because what software development should be *keeps changing*, and its changing so rapidly we can barely keep up, let alone actually settle on a bunch of standards. Instead, individual groups standardize themselves and you get a bunch of competing ecosystems each insisting they are right, without observing that they are all right at the same time - each standard is built to match the demands of not only *where*, but *when* it is being used.

Luckily, I don't program because I want to write programs all day. I program because I want to build something magnificent. If you make a living programming for a bunch of clients that try to tell you what to do and always say it should be faster and cheaper, well, [welcome to being an artist](http://clientsfromhell.net/).
