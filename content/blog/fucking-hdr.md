+++
blogimport = true
categories = ["blog"]
date = "2010-03-09T11:09:00Z"
draft = true
title = "Fucking HDR"
updated = "2011-01-22T04:17:08.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I really, really have to avoid thinking about HDR, because once I start working on one thing, I become completely and utterly *obsessed* with it. I'm talking an obsession more extreme then my love of bunnies. I'm talking an obsession that would have kept me up all night if I had let it. I've tried approximately 240 different equations in my head and none of them work. The approaches don't work. Nothing seems to work other then a few key things:

1. Bloom is subtle
2. Bloom is only visible under high contrast environments

Problem: How the ***fuck*** does one determine what exactly high contrast is?

3. IRL bloom behavior is a certain threshold below the brightness in question, and then anything getting too close to the original HDR brightness is completely eliminated.
4. It almost seems like you could just subtract the original source from the bloom, then somehow add this back into the calculation and do the HDR threshold calculation. This doesn't make sense mathematically though.

5. The IRL bloom tends to extend extremely far, but at the very edges its incredibly faint and just barely noticeable. This is stupidly hard to implement in a graphics pipeline because when you get things that faint they tend to just drop out completely instead of being subtle. if you try to use the inverse square law, squaring the values just removes the lower ones completely. I have been unable to find (for LDR anyway) an equation that preserves the subtle low values while throttling the high values. It might be that working with values of 0 to 1 is flawed and I though instead add 1 so that the lowest possible value is 1, thereby enabling division that makes sense. I implemented a version of this that did a really nice job of throttling the HDR values, but it still didn't seem to work very well for bloom.

6. I'm fairly confidant the key here lies in operating under HDR conditions by throwing out the assumptions normal bloom shaders use that are often used just to compensate for woefully inadequate spectrum analysis.

7. One thing that's really annoying is that I'm not entirely sure what I'm looking for each time I attempt to implement this. Furthermore most HDR pictures are corrupted by natural bloom which fucks with my own bloom if I try to use them. For an ideal testing scenario a full HDR pipeline must be implemented with proper HDR spectrum analysis for any of this to be effective. Attempting to put proper bloom on a traditional HDR crush algorithm will look stupid because the image has already been irreparably destroyed by the flawed HDR processing.

8. Note that the hilariously bad HDR example in the directX SDK has a natural bloom technique that, when not being grossly overused, is actually reasonably accurate in that it is circular and has a linear falloff. Ideally the falloff would be more exponential but that's proven to be stupendously difficult, so a subtler version of this would work really nicely.

9. *hidden*

I need to shut down my train of thought here before it overrides everything else (it's finals week for crying out loud), but hopefully the next time I become obsessed with this, it will be next year, about this time, when I have the proper environment to work in. When that happens I need to do some research on photographic HDR analysis, since I'm pretty sure they've figured out equations for a lot of the spectrum analysis.