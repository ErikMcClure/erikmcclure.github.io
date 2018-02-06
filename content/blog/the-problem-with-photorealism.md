+++
blogimport = true
categories = ["blog"]
comments = [1754883999439933588, 3991464566151358545, 1826994902732608946, 852334959237660961, 1040198884179304061, 2838416454355403429, 5004412755588436902, 1820172055925965399]
date = "2014-03-18T09:05:00Z"
title = "The Problem With Photorealism"
updated = "2014-03-23T03:46:25.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Many people assume that modern graphics technology is now capable of rendering photorealistic video games. If you define *photorealistic* as *any still frame is indistinguishable from a real photo*, then we can get pretty close. Unfortunately, the problem with video games is that they are not still frames - they *move*.

What people don't realize is that modern games rely on faking a lot of stuff, and that means they only *look* photorealistic in a very tight set of circumstances. They rely on you not paying close attention to environmental details so you don't notice that the grass is actually just painted on to the terrain. They precompute environmental convolution maps and bake ambient occlusion and radiance information into level architecture. You can't knock down a building in a game unless it is specifically programmed to be breakable and all the necessary preparations are made. Changes in levels are often scripted, with complex physical changes and graphical consequences being largely precomputed and simply triggered at the appropriate time.

Modern photorealism, like the 3D graphics of ages past, is smoke and mirrors, the result of very talented programmers and artists using tricks of the eye to convince you that a level is much more detailed and interactive than it really is. There's nothing wrong with this, but we're so good at doing it that people think we're a heck of a lot closer to photorealistic games then we really are.

If you want to go beyond simple photorealism and build a game that *feels* real, you have to deal with a lot of extremely difficult problems. Our best antialiasing methods are perceptual, because doing real antialiasing is prohibitively expensive. Global illumination is achieved by deconstructing a level's polygons into an octree and using the GPU to cubify moving objects in realtime. Many advanced graphical techniques in use today depend on precomputed values and static geometry. The assumption that most of the world is probably going to stay the same is a powerful one, and enables huge amounts of optimization. Unfortunately, as long as we make that assumption, none of it will ever feel truly real.

Trying to build a world that does not take anything for granted rapidly spirals out of control. Where do you draw the line? Does gravity always point down? Does the atmosphere always behave the same way? Is the sun always yellow? What counts as solid ground? What happens when you blow it up? Is the object you're standing on even a *planet?* Imagine trying to code an engine that can take into account all of these possibilities *in realtime*. This is clearly horrendously inefficient, and yet there is no other way to achieve a *true* dynamic environment. At some point, we will have to make assumptions about what will and will not change, and these sometimes have surprising consequences. A volcanic eruption, for example, drastically changes the atmospheric composition and completely messes up the ambient lighting and radiosity.

Ok, well, at least we have dynamic animations, right? **Wrong.** Almost all modern games still use precomputed animations. Some fancy technology can occasionally try to interpolate between them, but that's about it. We have no reliable method of generating animations on the fly that don't look horrendously awkward and stiff. It turns out that trying to calculate a limb's shortest path from point A to point B while avoiding awkward positions and obstacles amounts to solving the [Euler-Lagrange equation](http://en.wikipedia.org/wiki/Euler%E2%80%93Lagrange_equation) over an *n-dimensional manifold!* As a result, it's incredibly difficult to create smooth animations, because our ability to fluidly shift from one animation to another is extremely limited. This is why we still have weird looking walk animations and occasional animation jumping.

The worst problem, however, is that of *content creation*. The simple fact is that at photorealistic detail levels, it takes way too long for a team of artists to build a believable world. Even if we had super amazing 3D modelers that would allow an artist to craft any small object in a matter of minutes (which we don't), artists aren't machines. Things look real because they have a history behind them, a reason for their current state of being. We can make photorealistic CGI for movies because each scene is scripted and has a well-defined scope. If you're building GTA V, you can't somehow manage to come up with three hundred unique histories for every single suburban house you're building.

Even if we *did* invent a way to render photorealistic graphics, it would all be for naught until we figured out a way to generate obscene amounts of content at incredibly high levels of detail. Older games weren't just easier to render, they were easier to *make*. There comes a point where no matter how many artists you hire, you simply can't build an expansive game world at a photorealistic level of detail in just 3 years.

People always talk about realtime raytracing as the holy grail of graphics programming without realizing just what is required to take advantage of it. Photorealism isn't just about processing power, it's about *content*.
