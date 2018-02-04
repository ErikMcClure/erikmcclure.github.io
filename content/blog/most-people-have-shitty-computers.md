+++
title = "Most People Have Shitty Computers"
date = 2013-09-02T21:29:00Z
updated = 2013-09-03T16:54:53Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

{{%blockquote%}}*Premature optimization is the root of all evil* - Donald Knuth{{%/blockquote%}}

Ever since I started putting words on the internet, I have complained about the misinterpretation and overgeneralization of this Donald Knuth quote. Developers essentially use it as an excuse to never optimize anything, because they can always say they "aren't done yet" and then permanently render all optimizations as premature. The fact that "premature" is inherently a subjective term doesn't help. The quote was meant to target very low-level optimizations that are largely useless until you're positive everything else is working properly and you have no other low-hanging fruit to optimize.

Instead of following that advice, and only doing complex optimizations after all the low-hanging fruit has been picked, developers tend to just *stop optimizing* after the low-hanging fruit is gone. Modern developers will sometimes stop optimizing things *entirely* and leave it up to the compiler, which almost always does a horrible job at it. Because web development is inherently dependent on one of the [worst programming languages currently being used by anyone](http://www.ecmascript.org/), these issues are even more prominent.

When people complain about stuff not working on their phones while on shitty internet connections (which is every single free wifi hotspot ever), the developers tell them to stop using shitty internet connections. When they complain about how slow and unresponsive a stupid, unnecessary app whose entire purpose is to just *display text* (I'm looking at you, Blogger), the developers tell them to get a better phone. When someone writes an app that doesn't compress anything and sucks up bandwidth like a pig, for absolutely no reason at all, the developers tell them to get a better data plan.

**What the fuck?**

Just because you have a desktop with an 8-core processor and 32 gigs of RAM doesn't mean your customers do. The number of people using the top-of-the-line technology is a tiny fraction of the total number of people who actually use the internet, which at this point is [more than 1/3 the population of the entire human race](http://en.wikipedia.org/wiki/Global_Internet_usage). Only targeting hipster white kids who live in San Francisco and have rich parents may work for a small startup, but when Google's mail app turns into a piece of sludge that moves about as fast as [pitch at room temperature](http://en.wikipedia.org/wiki/Pitch_drop_experiment), we have crossed a line. Google is *everywhere*. Their stuff should work well on the shittiest internet connection you can imagine. You can't go around expecting all your customers to have perfect internet all the time when it's just not possible.

Don't just target the latest version of Windows and tell the other versions to stuff it, because it's almost never really necessary when all you're doing is using 3 or 4 new functions introduced in Windows 7 that could easily be encapsulated in a small check. Don't write your game in such a way that it will only be playable on a high end gaming machine because [you will get a lot of flak](http://en.wikipedia.org/wiki/Crysis#Game_engine). If making this happen is hard, you probably have a shitty game engine that has a bad architecture. Likewise, when my orchestral VSTi sucks up 10% of my CPU while it's just *sitting there* and *NOT ACTUALLY DOING ANYTHING*, something is seriously wrong. I know it's using the exact same sample format as Kontact (someone reverse-engineered it), except that it does everything 4 times slower. Yet, when people complain, and they complain all the time, the response is always "you need a better computer."

Most people have shitty computers. Your customers don't care about all the cool new features you can implement for your mail app now that you have time to work on them because you never bothered to optimize it properly - they just want a mail app that *WORKS*.
