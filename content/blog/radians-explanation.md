+++
blogimport = true
categories = ["blog"]
comments = [4190836184485292453, 6752032890881967991, 1545154667685345109]
date = "2011-07-06T17:22:00Z"
title = "Radians: An explanation"
updated = "2013-05-04T20:31:38.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Radians are used in almost everything that involves computers, and often come up in mathematical equations. However, most people are used to measuring angles in degrees, even though degrees (unlike radians) are a completely arbitrary measurement. To understand Radians, you need to understand **{{<math>}}\pi{{</math>}}**, or more precisely, why it *sucks*. Like a black hole, except <del>black holes don't actually *suck*, they just warp space-time so you fall into them</del> **OK BACK ON TOPIC:**

{{<math>}}\pi{{</math>}} is the ratio of a circle's *diameter* to its *circumference*. **This is retarded.** When was the last time you thought of a circle using its *diameter* for crying out loud? We define circles, and should intuitively think of circles using their *radius*, which is the distance from the center to any point on the edge of the circle.

{{<img height="350" width="350" src="http://upload.wikimedia.org/wikipedia/commons/thumb/1/1d/CIRCLE_1.svg/594px-CIRCLE_1.svg.png" alt="Circle">}}
{{<sup>}}<br/><i>source: <a href="http://en.wikipedia.org/wiki/File:CIRCLE_1.svg">wikimedia commons</a></i>{{</sup>}}

So that means that the ratio of the *radius* to the circumference is actually {{<math>}}2\cdot\pi{{</math>}}. This is why the formula for the circumference of a circle is {{<math>}}2\cdot\pi\cdot r{{</math>}}. In fact, its why we end up using {{<math>}}2\cdot\pi{{</math>}} almost everywhere, because we almost always define circles with the *radius*, but {{<math>}}\pi{{</math>}} is in relation to the *diameter*. Which is just stupid, and is in fact so stupid that some people are currently trying to define [{{<math>}}\tau{{</math>}} (Tau)](http://en.wikipedia.org/wiki/Tau) such that [{{<math>}}\tau = 2\cdot\pi{{</math>}}](http://tauday.com/). Sadly, {{<math>}}2\pi{{</math>}} still rules the world. I guess mathematicians really like pie...?

Either way, we must work within our constaints. So, keeping in mind that {{<math>}}2\pi{{</math>}} is the ratio between a circle's circumference and its radius, what happens if you have a unit circle (a circle with a radius of 1)? Well, that means it has a diameter of 2, so if you roll it out on the number line, you get this:

{{<img src="http://upload.wikimedia.org/wikipedia/commons/thumb/6/67/2pi-unrolled.gif/800px-2pi-unrolled.gif" alt="2 Pi unrolled" >}}
{{<sup>}}<br/><i>source: <a href="http://en.wikipedia.org/wiki/File:2pi-unrolled.gif">wikimedia commons</a></i>{{</sup>}}

When we talk about rotation and angles in *radians*, we are using the following relation:
{{<bmath>}} 360^{\circ} = 2\pi\;rad {{</bmath>}}That is, in *radians*, or {{<math>}}rad{{</math>}} as it is commonly abbreviated, a full rotation is {{<math>}}2\pi{{</math>}} because that's the circumference of a circle with a radius of 1. **Radius &rarr; Radians**. We know why its {{<math>}}2\pi{{</math>}} instead of just {{<math>}}pi{{</math>}}, because {{<math>}}pi{{</math>}} is stupid. If you want, you can say that {{<math>}}\tau = 2\pi{{</math>}}, so that {{<math>}}\tau{{</math>}} is then equal to the ratio between a circle's radius and its circumference. Then we get the much nicer relation:
{{<bmath>}} 360^{\circ} = \tau\;rad {{</bmath>}}360&deg; is just one rotation of a circle. A radian is simply how far around the circumference of a unit circle you want to go. So that means 180&deg; is halfway around the circle, which is {{<math>}}\tau/2{{</math>}} or just {{<math>}}\pi{{</math>}} radians.
{{<span style="font-size:150%">}}<div class="math">
<table><tr> <th style="padding-right:2em;"><b><span style="font-size:75%">Degrees</span></b></th><th><b><span style="font-size:75%">Radians</span></b></th> </tr><tr> <td>360&deg;</td><td>$$2\pi$$</td> </tr><tr> <td>180&deg;</td><td>$$\pi$$</td> </tr><tr> <td>90&deg;</td><td>$$\frac{\pi}{2}$$</td> </tr><tr> <td>60&deg;</td><td>$$\frac{\pi}{3}$$</td> </tr><tr> <td>45&deg;</td><td>$$\frac{\pi}{4}$$</td> </tr><tr> <td>30&deg;</td><td>$$\frac{\pi}{6}$$</td> </tr><tr> </table></div>{{</span>}}
This gives rise to the conversion functions:
{{<bmath>}} \begin{aligned}
rad = deg \cdot \left(\frac{\pi}{180}\right) \\
deg = rad \cdot \left(\frac{180}{\pi}\right) 
\end{aligned}
{{</bmath>}} But these simply arise out of {{<math>}}2\pi{{</math>}} being simplified:
{{<bmath>}} \begin{aligned}
rad = deg \cdot \left(\frac{2\pi}{360}\right) \\
deg = rad \cdot \left(\frac{360}{2\pi}\right) 
\end{aligned}
{{</bmath>}} So now we have an intuitive explanation of what radians are - **the distance around the circumference of the unit circle.**
