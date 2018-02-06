+++
blogimport = true
categories = ["blog"]
date = "2012-06-22T22:29:00Z"
title = "How Joysticks Ruined My Graphics Engine"
updated = "2013-05-10T01:59:52.000+00:00"
comments = [ 4511252302585754852, 7557298174961971984 ]
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
It's almost a tradition. 

Every time my graphics engine has been stuck in maintenence mode for 6 months, I'll suddenly realize I need to push out an update or implement some new feature. I then realize that I haven't actually paid attention to any of my testing programs, or their speed, in months. This is followed by panic, as I discover my engine running at half speed, or worse. Having made an infinite number of tiny tweaks that all could have caused the problem, I am often thrown into temporary despair only to almost immediately find some incredibly stupid mistake that was causing it. One time it was because I left the profiler on. Another time it was caused by calling the Render function twice. I'm gearing up to release the [first public beta](http://blackspherestudios.com) of my graphics engine, and this time is no different. 

I find an old backup distribution of my graphics engine and run the tests, and my engine is running at 1100 FPS instead of 3000 or 4000 like it should be. Even the stress test is going at only ~110 FPS instead of 130 FPS. The strange part was that for the lightweight tests, it seemed to be hitting a wall at about 1100 FPS, whereas normally it hits a wall around 4000-5000 due to CPU⇒GPU bottlenecks. This is usually caused by some kind of debugging, so I thought I'd left the profiler on again, but no, it was off. After stepping through the rendering pipeline and finding nothing, I knew I had no chance of just guessing what the problem was, so I turned on the profiler and checked the results. Everything seemed relatively normal except-  {{<pre>}}PlaneShader::cEngine::_callpresent       1.0    144.905 us   19%        145 us   19%
PlaneShader::cEngine::Update             1.0     12.334 us    2%        561 us   72%
  PlaneShader::cEngine::FlushMessages    1.0    546.079 us   70%        549 us   71%
{{</pre>}} ***What the FUCK?!*** Why is *70%* of my time being spent in {{<code>}}FlushMessages(){{</code>}}? All that does is process window messages! It shouldn't take any time at all, and here it is taking longer to process messages than it does to render an entire frame!  {{<pre cpp>}}bool cEngine::FlushMessages()
{
  PROFILE_FUNC();
  _exactmousecalc();
  //windows stuff
  MSG msg;

  while(PeekMessageW(&msg, NULL, 0, 0, PM_REMOVE))
  {
    TranslateMessage(&msg);
    DispatchMessageW(&msg);

    if(msg.message == WM_QUIT)
    {
      _quit = true;
      return false;
    }
  }
  
  _joyupdateall();
  return !_quit; //function returns opposite of quit
}
{{</pre>}} Bringing up the function, there don't seem to be many opportunities for it to fail. I go ahead and comment out {{<code>}}_exactmousecalc(){{</code>}} and {{<code>}}_joyupdateall(){{</code>}}, wondering if, perhaps, something in the joystick function was messing up? Lo and behold, my speeds are back to normal! After re-inserting the exact mouse calculation, it is, in fact, {{<code>}}_joyupdateall(){{</code>}} causing the problem. This is the start of the function:  {{<pre cpp>}}void cInputManager::_joyupdateall()
{
  JOYINFOEX info;
  info.dwSize=sizeof(JOYINFOEX);
  info.dwFlags= [ Clipped... ];

  for(unsigned short i = 0; i < _maxjoy; ++i)
  {
    if(joyGetPosEx(i,&info)==JOYERR_NOERROR)
    {
      if(_allbuttons[i]!=info.dwButtons)
      {
{{</pre>}}Well, shit, there isn't really any possible explanation here other than *something* going wrong with {{<code>}}joyGetPosEx{{</code>}}. It turns out that calling {{<code>}}joyGetPosEx{{</code>}} when there isn't a joystick plugged in takes a whopping 34.13 µs (microseconds) on average, which is almost as long as it takes me to render a single frame (43.868 µs). There's probably a good reason for this, but evidently it is not good practice to call it unnecessarily. Fixing this was relatively easy - just force an explicit call to look for active joystick inputs and only poll those, but its still one of the weirdest performance bottlenecks I've come up against. 

Of course, if you aren't developing a graphics engine, consider that 1/60 of a second is 16666 µs - 550 µs leaves a lot of breathing room for a game to work with, but a graphics engine must not force *any* unnecessary cost on to a program that is using it unless that program explicitly allows it, hence the problem. 

Then again, [calculating invsqrt(x)*x is faster than sqrt(x)](http://stackoverflow.com/questions/1528727/why-is-sse-scalar-sqrtx-slower-than-rsqrtx-x), so I guess anything is possible.
