+++
blogimport = true
categories = ["blog"]
date = "2010-05-08T00:48:00Z"
title = "Most Bizarre Error Ever"
updated = "2011-01-22T04:08:24.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Ok probably not the most bizarre error *ever* but it's definitely the weirdest for me.

My graphics engine has a Debug, a Release, and a special Release STD version that's compatible with CLI function requirements and other dependencies. These are organized as 3 separate configurations in my solution for compiling. Pretty normal stuff.

My example applications are all set to be dependent on the graphics engine project, which means visual studio automatically compiles the proper lib file into the project.

Well, normally.

My graphics engine examples suddenly and inexplicably stopped working in Release mode, but not Debug mode. While this normally signals an uninitialized variable problem, I had only flipped a few negative signs since the last build, so it was completely impossible for that to be the cause. I was terribly confused so I went into the code and discovered that it was failing because the singleton instance of the engine was null.

Now, if you know what a singleton is, you should know that this is absolutely, completely impossible. Under normal conditions, that is. At first I thought my engine instance assignment had been fucked, but that was working fine. In fact, the engine existed for one call, and then didn't exist for the other.

Then I checked *where the calls were coming from*. What I discovered next blew my mind.

One call was from Planeshader.dll; The other call was from *Planeshader_std.dll* - oh crap.

Somehow, visual studio had managed to link my executable to ***both*** instances of my graphics engine **at the same time**. I'm not entirely sure how it managed that feat since compiling two identical lib files creates thousands of collisions, but it appears that half the function calls were being sent to one dll, and half the function calls being sent to the other. My engine was trying to run in *two dlls simultaneously*.

I solved the problem simply by explicitly specifying the lib file in the project properties.

Surely my skill at breaking things knows no bounds.
