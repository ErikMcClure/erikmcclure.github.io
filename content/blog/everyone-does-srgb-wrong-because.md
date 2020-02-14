+++
blogimport = true
categories = ["blog"]
date = "2016-12-20T15:26:00Z"
title = "Everyone Does sRGB Wrong Because Everyone Else Does sRGB Wrong"
updated = "2016-12-22T07:03:44.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Did you know that CSS3 does all its linear gradients and color interpolation completely wrong? All color values in CSS3 are in the sRGB color space, because that's the color space that gets displayed on our monitor. However, the problem is that the sRGB color space looks like this:

{{<html>}}<center>{{</html>}}{{<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ef/SRGB_gamma.svg/250px-SRGB_gamma.svg.png" alt="sRGB gamma curve" >}}{{<html>}}</center>{{</html>}}

Trying to do a linear interpolation along a nonlinear curve doesn't work very well. Instead, you're supposed to linearize your color values, transforming the sRGB curve to the linear RGB curve before doing your operation, and then transforming it back into the sRGB curve. This is gamma correction. Here are comparisons between gradients, transitions, alpha blending, and image resizing done directly in sRGB space (assuming your browser complies with the W3C spec) versus in linear RGB:

{{<html>}}<table style="margin:0 auto"><tr><th>sRGB</th><th>Linear</th></tr><tr><td><div style="background:linear-gradient( 0deg, #FF0000, #00FF00);width:100px;height:100px;"></div></td><td style="vertical-align:center"><img src="/img/srgb_gradient.png" style="padding:0;border:0;"/></td></tr><tr><td><div id="srgbani" style="background:#FF0000;width:100px;height:100px;"></div></td><td><div id="linearani" style="background:#FF0000;width:100px;height:100px;"></div></td></tr><tr><td><img src="/img/srgb_radial.png" style="background:#FF0000;width:100px;height:100px;margin:0;padding:0;border:0;" /></td><td><img src="/img/srgb_alpha.png" style="padding:0;border:0;" /></td></tr><tr><td><img src="/img/srgb_resize.png" style="padding:0;border:0;" height="100" width="100" /></td><td><img src="/img/srgb_resize_correct.png" style="padding:0; height="100" width="100" /></td></tr></table>{{</html>}}
At this point you've probably seen a bazillion posts about how you're doing color interpolation wrong, or gradients wrong, or alpha blending wrong, and the reason is because... you're doing it wrong. But one can hardly blame you, because **everyone** is doing it wrong. CSS does it wrong because SVG does it wrong because Photoshop does it wrong because everyone else does it wrong. It's all wrong.

The amazing thing here is that the W3C is [entirely aware of how wrong CSS3 linear gradients are](https://lists.w3.org/Archives/Public/www-style/2012Jan/0635.html), but did it anyway to be consistent with everything else that does them wrong. It's interesting that while SVG is wrong by default, it *does* provide a way to fix this, via {{<code>}}color-interpolation{{</code>}}. Of course, CSS doesn't have this property yet, so literally all gradients and transitions on the web are wrong because there is no other choice. Even if CSS provided a way to fix this, it would still have to default to being wrong.

It seems we have reached a point where, after years of doing sRGB interpolation incorrectly, we continue to do it wrong not because we don't know better, but because everyone else is doing it wrong. So everyone's doing it wrong because everyone else is doing it wrong. A single bad choice done long ago has locked us into **compatibility hell**. We got it wrong the first time so now we have to keep getting it wrong because everyone expects the wrong result.

It doesn't help that we don't always necessarily *want* the correct interpolation. The reason direct interpolation in sRGB is wrong is because it changes the perceived luminosity. Notice that when going from red to green, the sRGB gradient gets darker in the middle of the transition, while the gamma-corrected one has constant perceived luminosity. However, an artist may prefer the sRGB curve to the linearized one because it puts more emphasis on red and green. This problem gets worse when artists use toolchains inside sRGB and unknowingly compensate for any gamma errors such that the result is roughly what one would expect. This is a particular issue in games, because games really do need gamma-correct lighting pipelines, but the GUIs were often built using incorrect sRGB interpolation, so doing gamma-correct alpha blending gives you the wrong result because the alpha values were already chosen to compensate for incorrect blending.

In short, this is quite a mess we've gotten ourselves into, but I think the most important thing we can do is give people the *option* of having a gamma correct pipeline. It is not difficult to selectively blend things with proper gamma correction. We need to have things like SVG's {{<code>}}color-interpolation{{</code>}} property in CSS, and other software needs to provide optional gamma correct pipelines (I'm looking at you, photoshop).

Maybe, eventually, we can dig ourselves out of this sRGB hell we've gotten ourselves into.

{{<html>}}<script>function decimalToHex(d) {   var hex = Math.floor(d).toString(16);   hex = "000000".substr(0, 6 - hex.length) + hex;    return hex; }  var start = null; function fromlinear(x) { return (x <= 0.0031308) ? (x * 12.92) : (1.055*Math.pow(x, 1/2.4)) - 0.055; } function transitionlinear(x) { if(!start) start = x; var delta = x - start; var v = document.getElementById("srgbani"); var v2 = document.getElementById("linearani"); var p = (delta*0.0005)%1.0; v.style.backgroundColor = "#" + decimalToHex(Math.floor(p*256)*256 + Math.floor((1-p)*256)*256*256); v2.style.backgroundColor = "#" + decimalToHex(Math.floor(fromlinear(p)*256)*256 + Math.floor(fromlinear(1-p)*256)*256*256); window.requestAnimationFrame(transitionlinear); } window.requestAnimationFrame(transitionlinear); </script>{{</html>}}
