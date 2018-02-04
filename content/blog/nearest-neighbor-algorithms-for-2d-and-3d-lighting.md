+++
blogimport = true
categories = ["blog"]
date = "2009-11-06T23:50:00Z"
title = "Nearest Neighbor Algorithms for 2D and 3D lighting"
updated = "2011-01-22T04:38:07.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
When doing lighting calculations for a graphics engine in either a 2D or a 3D context, one must find all the nearest objects within a given radius of a lightsource, as those are the only ones that you need to bother rendering things for. Done correctly, such an algorithm would allow for an arbitrarily large number of lights across an extremely large plane, with no real loss in preformance provided that no object was lit by more then say, 20 sources. As my physics-oriented friend pointed out, AI researches have been inventing ridiculously fast nearest neighbor algorithms for n-dimensional space for *decades*. The very thought of me even attempting to do my own little optimization of this is stupid when there's such astounding amounts of stupidly fast, free algorithms out there written by mathematical geniuses that can do it for me.

And yet, this is rarely, if ever, actually used in graphics. Graphics programmers seem to operate under the assumption that there are so few light sources (10 or less) in a given scene that there's therefore no point in implementing a superefficient light culling algorithm, since the gains could easily be transcended by improving the speed of the lighting calculation itself. While this theory holds water in most modern lackluster games, it seems to eschew the power of short-range lighting. Lots of very small lights can add a huge amount of atmosphere to a scene, and it doesn't need to be particularly computationally expensive as long as you cull the lighting radius to a set number of objects really really fast. The lighting calculations themselves, since they are only done on 4-5 objects at once as opposed to 500, are not an issue at all. At least, they aren't as long as your allowing for arbitrary n number of light sources.

Once again, a potentially helpful mathematical algorithm is completely ignored by the graphics community, despite its ease of use and obvious helpfulness.

*(I probably would have written more on this topic except i just got an idea for a pixel shader xD)*
