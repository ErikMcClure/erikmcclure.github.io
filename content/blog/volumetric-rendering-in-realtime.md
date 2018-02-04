+++
title = "Volumetric Rendering in Realtime"
date = 2010-01-16T14:48:00Z
updated = 2011-01-22T04:18:51Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

1. Do a depth render with additive blending, counterclockwise polygon culling
2. Do a depth render with additive blending, clockwise polygon culling.
3. Subtract 1 from 2 (or 2 from 1, whichever works), and you get precisely how much that pixel is "inside" the volumetric in question.

<a href="http://pics.livejournal.com/blackhole12/pic/00002rgc/"><img src="http://pics.livejournal.com/blackhole12/pic/00002rgc/s320x240" width="272" height="240" border="0" /></a>
