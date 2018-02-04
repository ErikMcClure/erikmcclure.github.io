+++
title = "Pixel Perfect Hit Testing"
date = 2010-08-17T06:27:00Z
updated = 2011-01-22T03:50:37Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

After beating World of Goo after stabilizing things in my game and renaming it, I wondered how easy it was to decompile C# applications and simultaneously thought this would be a great opportunity to get pixel perfect hit testing to work on my engine. So, I decompiled GearGOD's composition example and quickly discovered that his method of detecting mouse messages was... well something completely different then his extremely bad attempt at explaining it to me had suggested.

Basically, he did not run into the window event issues that I was having because... he didn't use them. XNA keeps track of the mouse coordinates in its own separate update function, most likely using its special input hook, and hence there is no mousemove to keep track of. Instead of occurring when the user moves the mouse, the hit tests occur *every single frame*. 

Hence, once you have utilized {{<code>}}WS_EX_TRANSPARENT|WS_EX_COMPOSITED|WS_EX_LAYERED{{</code>}} to make your window click-through-able, you then simply do a hit test on a given pixel after everything has been drawn, and swap out {{<code>}}WS_EX_TRANSPARENT{{</code>}} depending on the value. {{<code>}}GetCursorPos{{</code>}} and {{<code>}}ScreenToClient{{</code>}} will get the mouse coordinates you need, although they can be off your app window entirely so check for that too.

{{<pre>}}if(_dxDriver->MouseHitTest(GetMouseExact()))
  SetWindowLong(_window,GWL_EXSTYLE,((GetWindowLong(_window,GWL_EXSTYLE))&(~WS_EX_TRANSPARENT)));
else
  SetWindowLong(_window,GWL_EXSTYLE,((GetWindowLong(_window,GWL_EXSTYLE))|WS_EX_TRANSPARENT));{{</pre>}}
  
To get the pixel value, its a bit trickier. You have two options - you can make a lockable render target, or you can copy the render target to a temporary texture and lock that instead. The DirectX docs said that locking a render target is so expensive you should just copy it over, but after GearGOD went and yelled at me I tested the lockable render target method, and it turns out to be significantly faster. Futher speed gains can be achieved by making a 1x1 lockable render target and simply copying a single pixel from the backbuffer into the lockable render target and testing that.

{{<pre>}}void cDirectX_real::ActivateMouseCheck()
{
  if(_mousehittest) _mousehittest->Release();
  DX3D_device->CreateRenderTarget(1,1,_holdparams.BackBufferFormat,D3DMULTISAMPLE_NONE,0,TRUE,&_mousehittest,NULL);
}
bool cDirectX_real::MouseHitTest(const cPositioni& mouse)
{
  RECT rect = { mouse.x,mouse.y,mouse.x+1,mouse.y+1 };
  DX3D_device->StretchRect(_backbuffer,&rect,_mousehittest,0,D3DTEXF_NONE);

  if(mouse.x<0 || mouse.y < 0 || mouse.x > (int)_width || mouse.y > (int)_height)
    return false; //off the stage
  D3DLOCKED_RECT desc = { 0,0 };
  if(FAILED(_mousehittest->LockRect(&desc, 0,D3DLOCK_READONLY)))
    return true;    

  unsigned char color = (*((unsigned long*)desc.pBits))>>24;
  _mousehittest->UnlockRect();
  return color>_alphacutoff;
}
{{</pre>}}

Using this method, the performance hit is 620 FPS to 510 FPS at 1280x1024, which is fairly reasonable. However, my [Planeshader SDK example](http://www.blackspherestudios.com/storage/PlaneShader.zip) is still at 0.9.71, which does not have this updated, fast version, so it will be using a much slower method to do it. The end result is the same though.
