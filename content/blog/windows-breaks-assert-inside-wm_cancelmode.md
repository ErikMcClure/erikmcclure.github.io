+++
title = "Windows Breaks assert() Inside WM_CANCELMODE"
date = 2013-02-07T20:17:00Z
updated = 2013-02-07T20:17:29Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

So I have this dll that's doing a bunch of horrible WinAPI calls for me. It's designed to abstract away all the pain and unholy functions feeding on innocent blood. Little did I know that trying to cage WinAPI into a more manageable corner would be my undoing. 

The window is created and assigned a WndProc callback function inside this DLL, as per the various absurd requirements involving how you create windows in Windows. Everything works just fine and dandy until the application window suddenly closes, an error "ding" is heard, and the program silently crashes without any sort of error message whatsoever.

What just happened?

Well, I'm afraid you just triggered an assertion. But instead of doing what an assertion is supposed to do, which is immediately crash the program so you know what the fuck just happened, it instead somehow manages to create an infinite callback loop caused by the operating system actually trying to *assign the assertion dialog to the window*. You know, the same window that was part of an application that's supposed to be in the middle of *CRASHING*?! This causes another message to be sent, thus causing another assertion to be violated, et cetera until the stack overflows, except it doesn't actually tell you the stack overflows, it just crashes with absolutely no explanation. Even while in a debugger, which is truly amazing.

Specifically, the assertion triggers a dialog box to be displayed, but this dialog box forcibly sends another WM_CANCELMODE message to the application, apparently completely bypassing the entire point of having a message queue, because the program never gets to go back to it. It's WndProc is simply called with WM_CANCELMODE because *fuck you*, and so if your assertion fails when you're processing WM_CANCELMODE, it will simply do this over and over and over until the entire window tree collapses in on itself from the intense gravitational pull of *astronomical amounts of stupidity*.

The resulting black hole sucks in all nearby sources of sanity, which include some sort of error message about the stack overflowing, or perhaps the *actual fucking assertion error*, and then explodes, leaving you without a goddamn clue about what just happened. This is usually followed by copious amounts of screaming, then disbelief, then finally assuming the fetal position and praying for forgiveness for attempting to touch the windows API, as the Microsoft gods laugh and drag another developer's soul into their black circle of hell.

Somehow, every time I touch the windows API, it always ends with me curled into a ball, moaning "what the fuck" over and over again while crying.
