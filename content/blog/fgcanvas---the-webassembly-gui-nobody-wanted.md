+++
title = "fgCanvas - The WebAssembly GUI Nobody Wanted"
date = 2016-03-29T04:21:00Z
updated = 2016-03-29T04:21:30Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

As the gospel of WebAssembly descends upon the earth, offering salvation to the weary web developers of old, many are understandably excited. I'm excited too, primarily because I hate javascript with the fury of a thousand suns, and wish nothing more than to strap its burning carcass on to the hood of my statically-compiled-language-with-actual-type-safety car as I drive into the infinite abyss that is web development, screaming incoherent obscenities while plummeting towards my inevitable demise.

WebAssembly (or just wasm) answers the cry of app developers who really just want to treat the web as an abstract platform they can compile to, instead of compiling for desktop, mobile, and then erecting a shrine to satan inscribed with the runes of jQuery so you can summon the demons of HTML5 as you desperately try to create an interface that sorta-kinda looks like your desktop or mobile app. wasm turns the web browser into an abstract virtual machine, kind of like the JVM, except with fewer virgin sacrifices.



What really matters is that i'm releasing fgCanvas, a library guaranteed to piss off every web developer in a 50 mile radius of your computer. For this reason, it is recommended to use the library only during daylight hours, and to not use the library if you have any sort of open wound. In case a web developer does attempt to perform an exorcism on your hard drive, do not show weakness - they can smell fear.




This is probably a bad idea.

But has that ever stopped programmers? **No!** Our world runs on a single NPM package with 11 lines of code, we built all our security on an open-source library maintained by one guy in his spare time, and now we're building application GUIs on the desktop using a markup language intended for documents! Nothing gets in the way of a programmer who is determined to make his project work, especially not common sense!

And so, upon this bastion of madness, I have created a vessel to bury the last vestiges of sanity. Atop its bow, I laugh in bitter resignation as the flaming wreckage of the open web rains from a blood red sky, and the earth beneath me is sundered by endless chasms, torn apart by the wretched screams of tormented web developers.

Have I mentioned this is probably a bad idea?

Honestly, it'd be a considerably less bad idea if someone made something like [fgHTML](), which instead implemented the GUI elements by directly manipulating the DOM instead of just rendering everything on the canvas by taking advantage of the standardized JSON format that FeatherGUI uses to represent layouts. However, manipulating the DOM is a giant pain in the ass, and it completely misses the point of this blog post.

This is a warning. This is a glimpse into the inevitable future of web browsers that are essentially virtual machines that happen to have HTML parsers included. It doesn't matter if the blasphamous code in fgCanvas is purged from the earth by the holy fires of heaven itself, because another will pop up in it's place. With WebAssembly comes the promise of a web browser without javascript, or HTML, or CSS. HTML has sown the seeds of it's own defeat, because there is only so much you can do to a markup language for documents being horribly abused in an attempt to build application GUIs. It's a massive pain in the ass, and as long as something is a pain in the ass, developers will try to find an easier way to do it. Displaying GUIs the old fashioned way is a clear and obvious solution to their pain. Simply by mapping GUI renderers to use the HTML5 canvas or WebGL calls, old GUI toolkits can be ported to work on the web as if it were just another operating system.

The temptation to give into the sweet caress of the HTML5 canvas is simply too much. The age of scraping HTML will be over. Canvasapps will rule the day.
