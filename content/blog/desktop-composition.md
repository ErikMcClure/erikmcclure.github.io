+++
title = "Desktop Composition"
date = 2010-07-17T01:40:00Z
updated = 2011-01-22T04:06:59Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

I have succeeded in directly compositing my graphics engine to a vista window, using a dynamic DLL load so it doesn't cause any problems on XP. I also managed to create a situation where the window is entirely transparent and impossible to click, which was an absolute bitch to get working. For future reference, if you ever want a window that is click-through, use CreateWindowEx with both WS_EX_TRANSPARENT and WS_EX_LAYERED. Using only one will not work, nor will any sort of windows message handling. That is the ONLY WAY to make the window click-through. Consequently to do opacity based hittesting, you have to hook the mouse events for the entire desktop and manually notify your window after doing the calculations yourself (something I don't even want to think about).

Pics!

<a href="http://img340.imageshack.us/img340/905/compositewindow.png"><img src="http://img340.imageshack.us/img340/905/compositewindow.th.png" alt="" /></a>
<a href="http://img708.imageshack.us/img708/1072/compositingexample.png"><img src="http://img708.imageshack.us/img708/1072/compositingexample.th.png" alt="" /></a>

There are other options and you don't get to see the subtle adjustments I make to whether or not a window is draggable, or if its clickthrough etc, but it gives you a rough idea.

Now, on to stencil buffers and masking and maybe i'll release this thing only a week late! *FFFFFFFFFFFFFFFFFFF-*
