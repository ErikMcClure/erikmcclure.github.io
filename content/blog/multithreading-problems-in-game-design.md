+++
title = "Multithreading Problems In Game Design"
date = 2012-05-23T23:46:00Z
updated = 2012-05-24T02:21:40Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

A couple years ago, when I first started designing a game engine to unify Box2D and my graphics engine, I thought this was a superb opportunity to join all the cool kids and multithread it. I mean all the other game developers were talking about having a thread for graphics, a thread for physics, a thread for audio, etc. etc. etc. So I spent a lot of time teaching myself various lockless threading techniques and building quite a few iterations of various multithreading structures. Almost all of them failed spectacularly for various reasons, but in the end they were all too complicated.

I eventually settled on a single worker thread that was sent off to start working on the physics at the beginning of a frame render. Then at the beginning of each subsequent frame I would check to see if the physics were done, and if so sync the physics and graphics and start up another physics render iteration. It was a very clean solution, but fundamentally flawed. For one, it introduces an inescapable frame of input lag.

{{<html>}}<pre>Single Thread Low Load
&nbsp;&nbsp;FRAME 1&nbsp;&nbsp; +----+
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|
. Input1 -> |&nbsp;&nbsp;&nbsp;&nbsp;|
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|[__]| Physics&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|[__]| Render&nbsp;&nbsp;&nbsp;&nbsp;
. FRAME 2&nbsp;&nbsp; +----+ INPUT 1 ON BACKBUFFER
. Input2 -> |&nbsp;&nbsp;&nbsp;&nbsp;|
. Process ->|&nbsp;&nbsp;&nbsp;&nbsp;|
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|[__]| Physics
. Input3 -> |[__]| Render
. FRAME 3&nbsp;&nbsp; +----+ INPUT 2 ON BACKBUFFER, INPUT 1 VISIBLE
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;|
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;|
. Process ->|[__]| Physics
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|[__]| Render
&nbsp;&nbsp;FRAME 4&nbsp;&nbsp; +----+ INPUT 3 ON BACKBUFFER, INPUT 2 VISIBLE

Multi Thread Low Load
&nbsp;&nbsp;FRAME 1&nbsp;&nbsp; +----+
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;| 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;|
. Input1 -> |&nbsp;&nbsp;&nbsp;&nbsp;| 
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |[__]| Render/Physics START&nbsp;&nbsp;
. FRAME 2&nbsp;&nbsp; +----+&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
. Input2 -> |____| Physics END
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;|
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;| 
. Input3 -> |[__]| Render/Physics START
. FRAME 3&nbsp;&nbsp; +----+ INPUT 1 ON BACKBUFFER
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |____|
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;| PHYSICS END
.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp;&nbsp;| 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|____| Render/Physics START
&nbsp;&nbsp;FRAME 4&nbsp;&nbsp; +----+ INPUT 2 ON BACKBUFFER, INPUT 1 VISIBLE</pre>{{</html>}}

The multithreading, by definition, results in any given physics update only being reflected in the next rendered frame, because the entire point of multithreading is to immediately start rendering the current frame as soon as you start calculating physics. This causes a number of latency issues, but in addition it requires that one introduce a separated "physics update" function to be executed only during the physics/graphics sync. As I soon found out, this is a massive architectural complication, especially when you try to put in scripting or let other languages use your engine.

There is another, more subtle problem with dedicated threads for graphics/physics/audio/AI/anything. **It doesn't scale.** Let's say you have a platformer - AI will be contained inside the game logic, and the absolute vast majority of your CPU time will either be spent in graphics or physics, or possibly both. That means your game effectively only has two threads that are doing any meaningful amount of work. Modern processors have **8 logical cores**{{<sup>}}<a href="#ft_1">1</a>{{</sup>}}, and the best one currently available has **12**. You're using *two of them*. You introduced all this complexity and input lag just so you could use 16.6% of the processor instead of 8.3%.

Instead of trying to create a thread for each individual component, you [need to go deeper](http://inception.davepedu.com/). You have to parallelize each individual component separately, then tie them together in a single-threaded design. This has the added bonus of being vastly more friendly to single-threaded CPUs that *can't* thread things (like certain phones), because the parallization goes on at a lower level and is invisible to the overall architecture of the library. So instead of having a graphics thread and a physics thread, you simply call the physics update, then call the graphics update, and inside each physics and graphics update you spawn enough worker threads to match the number of cores you have to work with and concurrently process as much stuff as possible. This eliminates latency problems, complicated library designs, and it *scales forever*. Even if your initial implementation of concurrency won't handle 32 cores, because the concurrency is encapsulated inside the engine, you can just go back and change it later *without ever having to modify any programs that use the graphics engine*.

Consequently, don't try to multithread your games. It isn't worth it. Separately parallelize each individual component instead and write your game engine single-threaded; only use additional threads for asynchronous activities like resource loading.

{{<span style="font-size:80%">}}<br/><sup><a name="ft_1">1</a>{{</sup>}} The processors actually only have 4 or 6 *physical* cores, but use hyperthreading techniques so that 8 or 12 logical cores are presented to the operating system. From a software point of view, however, this is immaterial.</span>
