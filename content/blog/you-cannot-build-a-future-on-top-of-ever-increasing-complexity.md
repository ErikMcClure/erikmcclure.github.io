+++
title = "You Cannot Build A Future On Top Of Ever-Increasing Complexity"
date = 2014-05-10T20:19:00Z
updated = 2014-05-10T20:19:49Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Programmers love quoting Knuth out of context. "Premature optimization is the root of all evil." The keyword there being *premature*, not *all*. Too often this is used as an excuse to forgo optimization in lieu of new features, creating an endless deluge of terribly optimized programs that only run acceptably well on a grossly overpowered machine.

Another egregiously misused proverb is "don't re-invent the wheel." This is hammered into young programmers over and over until everyone is using massively over-engineered libraries to send cat pictures to each other. 

<img src="" alt="Wheels">

If we never re-invent the wheel, everyone has to use the same one. For some reason, some people think this is actually a good idea. They are apparently blind to the reality that if everyone has to use the same wheel, there are two possibilities. Either you'll have one big wheel that does everything, or wheels built on wheels built on wheels that wind up being an even bigger wheel that do everything. As each individual program introduces more subtle things it needs to do, the creed of never re-inventing a wheel means it will simply bolt all the features it needs on to some existing framework, *regardless of whether the original framework was designed for it*.

**You cannot build a future on top of ever-increasing complexity.** You can't simply keep putting more and more layers of abstraction on to a system and expect this to turn out well. Complexity begets instability and strange errors that simply compound with each added layer. Errors become more and more arcane until the source of the bug is buried so deep it takes just as long to figure it out as it'd take you to find a *memory corruption error*.

The abstractions we created to make our lives easier have grown so large and unwieldy they no longer make our lives easier, they just turn them into a different kind of hell. It ends up being easier to build a wheel that's designed to solve your specific set of programs. By only implementing a minimal set of features, you can reign in complexity and ensure the library stays both maintainable and *understandable*. Of course, the result of this is that you will have multiple libraries that do similar things instead of one library that does everything, and some people think this is bad.

Uh, ***NO***.

You *want* there to be a few libraries out there that do the same thing, or everyone will be using the exact same library, and there will be no competition. Do you know what happens when everyone is using the same library? The [heartbleed bug](http://en.wikipedia.org/wiki/Heartbleed) happens, and then suddenly half the internet is vulnerable. Are the purists happy now? Almost everyone was using the exact same code, just like you wanted, and the result was *one of the most catastrophic security flaws in modern history*.

Without competition, a software library will stagnate, just like in an economy. Without competition, there is no evolution, and without evolution, there is no progress.
