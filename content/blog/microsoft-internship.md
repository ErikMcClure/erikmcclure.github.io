+++
blogimport = true
categories = ["blog"]
date = "2012-07-21T07:56:00Z"
draft = true
title = "Microsoft Internship"
updated = "2013-12-19T17:16:00.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
For me, watching Microsoft for the past 5 years has been a lot like getting on a train, immediately getting off at the next stop, only to watch it explode 5 minutes later. You see, I was a high school intern there back in the summer of 2008, right before my senior year of high school. What I witnessed there I will never forget for the rest of my life, and continue to consider it the gold standard of how to not run a software development company. I live 10 minutes from Microsoft HQ in Redmond, Washington, and ever since I learned how to program I thought I wanted to work there. Until I actually did.

**Bloatware**
I was assigned to a testing team whose job was to write automated tests and do other test-related work on a certain product. This product is, to this date, the most horrifying monstrosity I have ever seen in programming. If you tried to cold run the debug build without any optimization, it took **20 MINUTES**, on average, to start up. I actually tried it once just to see if it would really happen (it actually only took 18 minutes on my particular computer). I don't know what the fuck it was doing, but it was clearly a disaster.

They managed to get this down to ~3 minutes by doing some weird dll-binding trick that shouldn't exist. Compiling to release mode under full optimization got it down slightly below a minute. This is perhaps the part that disturbed me most of all - I can see taking 20 minutes to load up some stupid amount of data for an equally stupid reason, but 20 minutes to load up *DLLs? **CODE?*** But when they invented 2 useless programming languages for this specific project for no damn reason, I suppose that was somewhat inevitable. 

Did I mention that the project is written in like 5 different languages, two of which were created specifically for it? They were all managed, of course, but that didn't stop the program from leaking memory out of its ears. When I was brought on, there were some very severe memory leaks from moving a window around, somehow, and they had only just managed to *reduce* the leak from something truly horrifying to a more manageable 4 MB/hour or something to that effect. Forget trying to *fix* memory leaks, the best they can do, when using a bunch of *managed languages*, is just reduce the amount of memory being thrown into the shredder. It was unspeakably bad.

**Meetings**
Microsoft is infamous for its ridiculous meetings, and for good reason. We had meetings roughly every other day. My team leader mentioned he wished he could code more, but almost all his time was spent at meetings. I had to learn to use Outlook just to organize when each thing was where doing what. We did occasionally have meetings on when to have meetings. They were always astonishingly boring. One particular event, however, stood out above all the rest.

One fateful day, we had a meeting where one of the *major discussion points* on the to-do-list was that a function name was more than 50 characters long. No, I'm not kidding. They first discussed "Gee, is this name too long? Is 50 characters too long or can we get away with it? Should we just try to change it first?" Then they spent about 10 minutes *trying to figure out what to rename this stupid function to* as I'm sitting here now seriously considering starting my own company because my god I couldn't fuck it up this bad if I *tried*.

Of course, every now and then I got to tag along to a higher profile meeting instead of simple end-of-week team meetings, where my superior's superiors would be talking to their superiors. Did you know they had an entire program just for figuring out who reported to who? This was back when Bill Gates was still sitting at the top of the pyramid, to whom only Steve Balmer reported to. At the time, it seemed pretty silly, but now that I've seen Steve Balmer drive Microsoft off a cliff after Bill Gates left, perhaps that solitary, lonely line indicating Balmer reporting to Gates actually *did* matter.

Naturally, as a result of this, there were protocols in place for when someone was out sick for whom to escalate issues to. If my team leader was gone, everyone below him was to report directly to boss of our team leader, and if that guy was gone, everyone below him reported to the next guy up, and so on and so forth. For one week, both my team leader and his superior were both gone, so I was actually reporting to my superior's superior's superior. It was during this week I got to visit a meeting between department heads and other very important people, because my superior's superior's superior thought it would be interesting (it was, for all the wrong reasons). But during that week, there was a 3 hour period when my superior's superior's superior was actually gone. They weren't really sure what to do about that, since at that point I would technically have to report any problems to the *department head*, or my superior's superior's superior's superior.

I was really careful not to break anything for 3 hours.

**The Managed Cult**
I was subjected to multiple code reviews. At almost all of them, they thought my code was very good, except in a lot of places it was *too clever*, or in at least one case, *too elegant* (no seriously). So I had to keep writing clearly subpar code in case I "got hit by a truck" and Mr.Dumbfuck was assigned to maintain my program. Every single time I was told this I couldn't help but wonder why they kept hiring dumbasses in the first place. I couldn't use any interesting features of C# to craft elegant, rock-solid solutions, I had to do everything the hard, stupid way.

The irony of this is that the project I was working on was, by definition, using C# in a way it was not designed for. You see, it was supposed to be tying in to some low-level windows metric-analysis API. I thankfully inherited a large part of the actual API interface from someone else's attempt, and naturally it was basically an enormous hack. Fully half the program was simply PInvoke and structs using insane alignment hacks to mimic unions. In one case, the documentation for the marshaling was wrong. In another case, the function description for the native API was wrong. In the case I had to deal with, the function itself didn't actually do what the documentation said it did.

So my project was one giant hack, and in my code reviews I was forced to use simplistic coding practices. Of course, at the time, I had been learning C++ for several months, and whenever any C++ idioms snuck into my code, I would be sternly chastised and reminded that "C++ is a bad language", which really nailed home for me the fact that these programmers simply couldn't fathom a world outside of Microsoft and Windows and the neat little safe playground that had been constructed for them. Obviously, C++ was a bad language, but that won't stop us from desperately using C# in ways it should never, ever be used.

Near the end of my internship as I was digging into deeper and deeper metrics, I eventually found out that the thing they had asked me to do was actually **completely impossible**. Certain critical API calls for an entire class of metrics were simply not available to non-native code. At all. No PInvoke, no API calls, no workarounds, nothing. So in perhaps one of the most hilarious revelations I have ever been part of, the team realized the project *had* to be rewritten in C++.

Oops.

**2009**
After graduating high school and *just barely* being accepted into the UW by some cosmic stroke of luck, I once again applied to the internship program. I halfheartedly requested to be put in something having to do with DirectX or graphics or high-performance computing, but when I stepped into the interview, I was told I'd be working in the Office division. Despite managing to figure out their retarded brain-teaser question which had absolutely nothing to do with how well I coded, I couldn't bring myself to care. Was this incredibly boring, well-paying job what I really wanted?

I lost the position to a close friend, and that was the end of my Microsoft career. I was secretly relieved, and used the opportunity to throw myself at my stupid little 2D graphics engine. By the time applications for college internships were due, I had realized that any sort of normal programming job just wasn't for me. Years later, it become apparent that I had narrowly avoided a catastrophic trainwreck.

Then I was rejected from the UW computer science major. But that's another story.
