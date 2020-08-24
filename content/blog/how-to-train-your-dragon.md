+++
blogimport = true
categories = ["blog"]
date = "2010-10-18T04:42:00Z"
title = "How To Train Your Dragon"
updated = "2011-01-22T03:44:20.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
...takes its place as one of my favorite movies of all time. The characters are brilliant, the story is superb, the music is divine, and the cinematography is breathtaking. It was so good it gave me the last bit of inspiration I needed to make [this (WIP)](http://soundcloud.com/erik_mcclure/inmydreams), and as a result my attempts at finishing CEGUI this weekend were thrown out the window. Tomorrow I'll try a first draft of the drums and possible a couple of effects before extending the main chorus. These new samples are amazing - it's like I can do everything I ever wanted. Now the hard part is figuring out what I want. I threw in a bunch of binaural sounds thanks to [the free sound project](http://www.freesound.org), which has been added to my mental list of useful resource sites.

I finally got the entire CEGUI dependency chain statically recompiled and have wrapped everything into a giant megadll with no dependencies. I then discovered that for all this time, PlaneShader's Release mode had been defaulting to __fastcall. I have no idea how it has even been compiling with my other projects; the reason I don't change the calling convention is because it breaks absolutely everything. For good measure I threw a few extra BSS_FASTCALL into the really high traffic functions.

So, if it weren't for my sudden musical inspiration, I'd be rebuilding the GUI in CEGUI, although I also need to figure out how the heck it renders text, because I need a text renderer, and I can't just require CEGUI's to be used. I need to build a very basic but supercrazyfast text renderer, because holy shit is directX bad at that.

After that I finalize a midpoint release of planeshader and move to reimplementing Decoherence's GUI in CEGUI, then FINALLY get to those joints that were supposed to be done ages ago.

Of course, I also have a midterm wednesday. Fuck.
