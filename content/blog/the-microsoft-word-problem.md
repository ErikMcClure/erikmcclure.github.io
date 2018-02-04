+++
blogimport = true
categories = ["blog"]
date = "2013-09-18T22:34:00Z"
title = "The Microsoft Word Problem "
updated = "2013-09-28T07:12:36.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I use FL Studio 11 to make my music. Some people find this surprising, due to FL Studio's stigma as being a "toy" DAW that it simply cannot seem to shake, despite the fact it actually does most things as well or better than other professional DAWs. FL Studio has two real, significant issues: Large projects are unstable, and it has extremely bad 64-bit support.

This is almost never what people actually complain about. Instead, people complain about FL studio lacking features when they really just need to enable the right option. My friend lamented FL Studio's poor handling of automation clips without knowing about the "browse parameters" feature that lists all of a synth's parameters and highlights the one you are changing. He then complained that it didn't play notes when you started playback in the middle of them - this can be toggled in the audio settings. He also didn't know how to group multiple instruments to be controlled by the same piano roll, so I had to show him how to use Layers. Every single reason he gave for FL Studio being crap was just him not understanding how to use it properly, or not knowing where to find a specific feature.

This is the *exact same problem* Microsoft had with Word, and it's what resulted in the Ribbon GUI overhaul. Every reason people gave for not liking Word was just a result of them not being able to find the feature they wanted in an endless cascade of menus. To address this, it required a GUI overhaul so those features were grouped in intuitive ways, so people could find them more easily.

Even APIs can suffer from the Microsoft Word Problem. OpenGL, especially it's early incarnations, was essentially a giant, opaque list of functions that randomly referenced each other and had no clear organization. The [online documentation](http://www.opengl.org/sdk/docs/man/) is literally just an alphabetized list of every single OpenGL function in existence. The end result is that you need to know precisely what you want to do in order to find the right function. This is in contrast to DirectX, which (for example) has every single renderstate in a [single, enormous enumerator](http://msdn.microsoft.com/en-us/library/windows/desktop/bb172599(v=vs.85).aspx). That enumerator contains something like 50 or so equivalent OpenGL function calls, which are not grouped in any meaningful way. It's documentation, like everything else on MSDN, is organized in hierarchical groups. DirectX tells you every single possible thing you theoretically might be able to do with your graphics card. OpenGL, on the other hand, has extensions, which are both a blessing and a curse. It makes OpenGL inherently more adaptable than DirectX, but at the cost of having absolutely no idea what the graphics card may or may not be capable of, because it's just *not in OpenGL*, it's in some extension you have to dig up.

Just like Microsoft Word, the features are all there in OpenGL, they're just harder to get to. And you'd better hope you know exactly what your technique is called, or you'll never find the function you want. WebGL inherits this problem, and throws on a few of its own inherent with attempting to bolt a low-level C api on to a high level web language that needs to be sandboxed.

We can't simply implement a feature, we have to make it easy to find, and easy to use, or it's worthless. It's amazing how quickly programmers forget the importance of *context*. In all of these cases, the solution to feature overload is conceptually very similar - Word created the Ribbon, which grouped related features under tabs and sub-groups. A similar grouping of OpenGL functions and parameters would do wonders for it's usability. FL Studio would also benefit by dumping it's left-hand browsing bar in favor of a Ribbon or module approach that didn't force 15 wildly different GUI styles into a single menu.

Context is important. If I want to find the function that sets the alpha blending operation in OpenGL, I should not have to go through over 300 functions to find it. I should be able to go to the "sampling" functions and look in the "blending" subgroup, and poof, all the functions that have anything to do with blending are there. It doesn't matter if we're designing word processors, digital audio workstations, low level APIs, middleware, or web applications. Anything and everything is susceptible to the Microsoft Word Problem.
