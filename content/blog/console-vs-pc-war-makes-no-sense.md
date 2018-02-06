+++
blogimport = true
categories = ["blog"]
comments = [7171091385926323649, 4989085578820151619]
date = "2011-04-23T20:16:00Z"
title = "Console vs PC War Makes No Sense"
updated = "2011-04-23T20:28:29.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I'm a software engineer. Specifically, my specialty is writing all the code that makes games work. Every time I see someone say "I'm a console gamer" or "I'm a PC gamer" I want to punch them in the face.

**THERE IS NO DIFFERENCE BETWEEN A CONSOLE AND A PC**.

Let me explain why in cold, hard facts. From a development perspective, consoles are unique from PCs in that they usually have specialized controllers, and all the hardware is perfectly consistent between players, and they usually have fairly powerful hardware specifically designed to benefit games. When one is developing for a PC, on the other hand, you have to account for a keyboard, mouse, joystick, multiple USB controllers, 10 year old graphics hardware, graphics hardware that *hasn't even come out yet*, the internet, multiple CPU architectures, 3 versions of directX, plus openGL, and at least 3 different operating systems.

The simple fact is that, from a purely developmental perspective, a *PROPERLY DESIGNED ENGINE* can treat a console as simply another platform, with its own operating system, CPU architecture, and graphics API, since a proper engine will need to be able to account for multiple OSes, architectures, and APIs in a normal PC market *anyway*. Not only that, but unless you are a mentally retarded programmer, it should be cake to put in architecture specific optimizations for each console, even ones with significant pipeline differences.

A lot of programmers, for some reason, don't think this is possible. I think that's hilarious. Let's take a PS3 Cell Processor versus a Pentium 4 versus an i7 quad-core. Since modern consumer PCs can reach up to 12 separate logical cores, if your modern graphics engine doesn't have a job queue for parallelized tasks, you are an idiot. Furthermore, since a job queue can utilize functors and accept pretty just any generic function if properly done, its interface can be totally abstracted out of the engine. Consequently, on a pentium 4, the job queue can probably just be configured to immediately process everything on a single thread, or whatever is appropriate. Then, for the Cell and quad-cores, its just a matter how many cores are available. Since the data structure is completely abstract, not only can you optimize for the core-count, but you can optimize down to assembly-level code for optimal performance for both the PS3 cell processor and the quadcore. This holds even if they have vastly different resource requirements, because your data structure should be aware of those limitations anyway. The most you'll have to do is tweak your effects to balance the load on each platform, which *you should be doing anyway*. Hence, a single engine can support all consoles and PC platforms with little, **if any** performance loss. As an extension of this, the next time I hear derogatory comments on the line of "its just a PC port" or "its just a console port" I'm going to murder a kitten, because *that's just lazy programming*, god fucking damn it.

So, technically speaking, a console is just a **pre-configured, standardized PC**. Consequently anyone who says consoles have better hardware is being a complete ass-backwards moron. However, some people seem to think that certain pre-configured PCs are better then other pre-configured PCs. The only valid argument here is one that compared the economic cost of each individual component, which rarely happens. Instead they usually get into fighting matches about game selection, online components, and comparing graphics hardware power, which to me seems only slightly less childish then comparing penis sizes.

First, as I have just explained, any properly programmed PC game should be able to run on just about any console, no matter what, or they are lazy programmers. None of this has anything to do with the console itself, and everything to do with either bad programmers or braindead middle management. So if you guys want to be like "MY MIDDLE MANAGEMENT IS BETTER THEN YOUR MIDDLE MANAGEMENT" then, um, you do that. The online component is even dumber, because it really comes down to, where are your friends? If you value an online component, its because it lets you play with your friends. If none of your friends have a PS3, then its online component is going to suck total balls no matter how awesome everyone says it is. Criticizing someone for getting a console because they're friends have it so they can play online together is like telling someone they're stupid because they bought the same game their friends did so they can play together.

What's really funny is that you can actually argue that a console's unique control system, like the Wii, or the Kinect, could have a significant impact on a game since it would have to structure its gameplay based on a controller that is, for the most part, not widely available on PCs (although both of those controllers can and will work with a PC). The irony here is that both of these systems get trash-talked by the hardcore gamers as being even *less* hardcore then a normal controller! So the only valid argument for consoles is being used by hardcore console gamers to deride casual players, which is just comical.

The only other thing I can fathom these gamers arguing over is that console gamers are more hardcore then PC gamers.

This is what I hear: "I'm a better person then you because I spend more money on games!"

Wow. Lets just all start persecuting each other over our religions now, because why the fuck not, it seems those insane gamers have all but done this already. Seriously, if I, as a game developer, make a game for a PC, the only reason its not going to end up on a console is because the distributors are being assholes, and that does not give anyone a right to say that its not a game for "hardcore" gamers simply because some fucker with a business suit decided it would threaten their moneymaking monopoly.

So, if you ever do say that, ***I will find you***.
