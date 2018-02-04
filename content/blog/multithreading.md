+++
title = "Multithreading"
date = 2010-04-10T15:14:00Z
updated = 2011-01-22T04:14:02Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

This was a mistake. I have learned several things about multithreading ever since I started attempting to restructure my engine around it. Recently I have just discovered that *everything I did with multithreading was wrong*. Lockless threading was invented for being applied to realtime applications. In addition, Windows has a serious issue with this kind of stuff, and so the best library out there only works on Mac and Linux. All the windows lockless thread are written in C# because they love the garbage collector.

Thus, even beginning to start with implementing lockless stuff would require me to multithread my memory management, my utilities, my graphics engine, my audio engine, and rewrite pretty much every single line of code I've written over the past 4 years. And then I'd have to *test all of it*.

So instead of making everything threaded, I have ended up having to remove all threads completely. I know that somewhere, somehow, a bunch of programmers are going to laugh at my poor, single-threaded game, but unless I had like 6 other programmers to help me out here, I have no choice. I've learned a lot about priorities, and I know not to do stupid things simply for the sake of being modern. Luckily, this is a 2D graphics engine, so the vast majority of the stress put on the CPU is the physics. Because of this there may be a way for me to evaluate physics while I'm rendering the graphics and doing game logic. If I can just get the physics into another thread, even if its a stop-and-go kind of thing, it would give me all the performance bonuses I need from the second CPU core without needing super-duper complex lockless architectures and god knows what else. It may also be possible to multithread the network update packets by taking advantage of the update function in the cReal objects and forcing them to wait until they are about to sync all their physics data to actually update the information. By carefully structuring the update functions, it is possible to achieve fairly high performance without building an entire goddamn library.

Note to self: Update each physics step with a delta value from the graphics engine.
