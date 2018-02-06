+++
blogimport = true
categories = ["blog"]
comments = [207165470266513944, 6058470627287172164, 3387194789355186621, 6731734577867089647, 9085081330358356594, 7625115543485866866, 3578656417029319459, 3297418448319511209, 4862435661967305292, 6739184957295880987, 8599676099757838759, 8652249131600652504, 8838090052083035302, 5858170268164430638, 195838438836619429, 7579093576559208910, 6750500124568624163, 90238492610716238]
date = "2012-09-19T07:33:00Z"
title = "Analyzing XKCD: Click and Drag"
updated = "2012-09-19T08:03:43.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Today, xkcd [featured a comic](http://xkcd.com/1110/) with a *comically* large image that is navigated by clicking and dragging. In the interests of SCIENCE (and possibly accidentally DDoSing Randall's image server - sorry!), I created a static HTML file of the **[entire composite image](http://blackspherestudios.com/storage/xkcd_huge_static.html)**.{{<sup>}}1{{</sup>}}

The collage is made up of 225 images{{<sup>}}2{{</sup>}} that stretch out over a total image area 79872 pixels high and 165888 pixels wide. The images take up 5.52 MB of space and are named with a simple naming scheme {{<code>}}"ydxd.png"{{</code>}} where d represents a cardinal direction appropriate for the axis (n for north, s for south on the y axis and e for east, w for west on the x axis) along with the tile coordinate number; for example, {{<code>}}"1n1e.png"{{</code>}}. Tiles are 2048x2048 png images with an average size of 24.53 KB. If you were to try and represent this as a single, uncompressed 32-bit 79872x165888 image file, it would take up 52.99 GB of space.

Assuming a human's average height is 1.8 meters, that would give this image a scale of about 1 meter per 22 pixels. That means the total composite image is approximately 3.63 kilometers high and 7.54 kilometers wide. It would take an average human 1.67 hours to walk from one end of the image to the other. Note that the characters at the far left say they've been walking for 2 miles - they are 67584 pixels from the starting point, which translates to 3.072 km or ~1.9 miles, so this seems to indicate my rough estimates here are reasonably accurate.

If Randall spent, on average, one hour drawing each frame, it would take him 9.375 days of constant, nonstop work to finish this. If he instead spent an average of 10 minutes per frame, it would take ~37.5 hours, or almost an entire 40-hour work week.

Basically I'm saying Randall Munroe is fucking insane.

{{<span style="font-size:80%">}}{{<sup>}}1{{</sup>}} If you are on firefox or chrome, right-clicking and selecting "Save as" will download the HTML file along with all 225 images into a separate folder.
{{<sup>}}2{{</sup>}} There are actually 3159 possible images (39 x 81), but all-white and all-black images are not included, instead being replaced by either the default white background or a massive black &lt;div&gt; representing the ground, located 28672 pixels from the top of the image, with a height of 51200.
{{</span>}}
