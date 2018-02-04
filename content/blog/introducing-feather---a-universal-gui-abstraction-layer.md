+++
title = "Introducing Feather - A Universal GUI Abstraction Layer"
date = 2013-12-17T23:03:00Z
updated = 2013-12-17T23:03:56Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Implement for:
Windows/Mac/Linux
Games (SDL/LÖVE/PlaneShader/Ogre3D/irrlicht)
HTML/CSS using asm.js with a backend that renders using HTML elements
VSTi
Linux window manager based on TinyWM.c



[Feather]() is a lightweight GUI abstraction layer that allows you to build a GUI layout and make it work on *anything*, even **games**. It's [open-source](), extensible and unlike every other GUI toolkit in existence, actually has an editor that lets you easily [build your own skins](), in addition to layouts. Feather does not force you to do anything - the core library itself has no dependencies whatsoever and is written in C, so you can build bindings to any programming language you want. Feather is currently implemented in [SDL](), [LÖVE](), [PlaneShader](), [WinAPI](), [GTK+](), [Ogre 3D](), [irrlicht](), [Unity 3D](), and [XNA]().

Unlike all the other GUI toolkits, Feather *only* concerns itself with being a GUI. Serialization can be done with either protocol buffers or XML using an optional library extension. Rendering is done by *implementations* of the abstraction layer. While a bare-bones implementation only has to render images and text, the provided WinAPI implementation overrides all the predefined widgets and wraps them in true windows controls that can mimic the current operating system theme like a real windows application would. Because it properly implements Feather, if you want to override the default look, it will magically render all possible Feather skins on the windows controls themselves.

But Feather is an *abstraction layer*, which means we can do some really weird things with it. That's why I've also included a GTK+ implementation. Yes, I implemented my GUI toolkit using another GUI toolkit, because Feather is *just that awesome*. This GTK+ implementation can be used on Linux and Mac applications (but I've only tested it on Fedora).

Feather is built on top of a messaging system. Most things are done by sending messages to either GUI windows or static renderables (images/text/etc). The underlying system is implemented using C-style single inheritance. Due to this messaging system, responding to events comes down to overriding the GUI system's message hook. You can then sort the incoming GUI window by ID and then by message type to determine what action you should take. This message-oriented design was chosen because its very easy to extend this to work nicely with other languages. Included in the SDK is a C++11 lambda extension that uses a perfect hash of IDs mapped to an array of functions, one to handle messages for each unique GUI widget. Similar techniques can be used to map it to python functions, lua functions, etc.

Here are some screenshots:



A prebuilt SDK is available for Windows, which also contains the example application. This application uses some dynamic DLL loading trickery to let you switch between GUI implementations on the fly, which is a demonstration of just how absurdly powerful this abstraction layer is. If you haven't noticed, its a breeze to integrate into a game engine; I was able to integrate it into *7 separate* 2D and 3D graphics engines in a matter of days. Feather is still in beta, seeing as I only started developing it 2 weeks ago, and so probably still has a fair number of really stupid catastrophic bugs lurking around. If you find any, please [tell me]() so I can get them fixed, or fix them yourself and append the fix to the bug report. This is my first time using Git for a project, so I have no idea how to do pull requests or how that would even work on google code, sorry!

**But Why?** Feather is a product of rage and frustration. After years of struggling with CEGUI's horrendous, bloated lasagna code only to find it woefully inadequate for my needs, I decided I needed a better solution. But I was simultaneously dealing with GTK+'s catastrophic configuration nightmare and horrible, horrible editors. It wasn't until I realized I could solve both problems at the same time that I began work on Feather. Feather is lightweight because I am absolutely sick of all these GUI toolkits trying to do absolutely everything{{<sup>}}1{{</sup>}} instead of just *being a GUI toolkit*. Feather does exactly one thing, does it well and **just works**. It also serves as the missing link between in-game GUIs and native GUIs by allowing them to co-exist on a higher level of abstraction. Now if I need one of my native client editors to be in-game, I can just substitute an implementation. CEGUI said that game developers should be making cool games, not building GUI subsystems. I say let game developers make their own damn GUIs.
