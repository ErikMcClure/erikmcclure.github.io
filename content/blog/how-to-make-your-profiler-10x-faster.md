+++
title = "How To Make Your Profiler 10x Faster"
date = 2014-06-28T17:31:00Z
updated = 2017-04-09T16:51:59Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Frustrated with C profilers that are either so minimal as to be useless, or giant behemoths that require you to install device drivers, I started writing a lightweight profiler for my [utility library](http://bss-util.blackspherestudios.com). I already had a high precision timer class, so it was just a matter of using a radix trie that didn't blow up the cache. I was very careful about minimizing the impact the profiler had on the code, even going so far as to check if extended precision floating point calculations were slowing it down.

Of course, since I was writing a profiler, I could use the profiler to profile itself. By pretending to profile a random number added to a cache-murdering int stuck in the middle of an array, I could do a fairly good simulation of profiling a function, while also profiling the act of profiling the function. The difference between the two measurements is how much overhead the profiler has. Unfortunately, my initial results were... *unfavorable*, to say the least.
{{<pre>}}BSS Profiler Heat Output: 
[main.cpp:3851] test_PROFILE: 1370173 µs   [##########
  [code]: 545902.7 µs   [##########
  [main.cpp:3866] outer: 5530.022 ns   [....      
    [code]: 3872.883 ns   [...       
    [main.cpp:3868] inner: 1653.139 ns   [.         
  [main.cpp:3856] control: 1661.779 ns   [.         
  [main.cpp:3876] beginend: 1645.466 ns   [.         
{{</pre>}}The profiler had an overhead of almost *4 microseconds*. When you're dealing with functions that are called thousands of times a second, you need to be aware of code speed on the scale of *nanoseconds*, and this profiler would *completely ruin* the code. At first, I thought it was my fault, but none of my tweaks seemed to have any measureable effect on the speed whatsoever. On a whim, I decided to comment out the actual _querytime function that was calling QueryPerformanceCounter, then run an external profiler on it.
{{<pre>}}Average control: 35 ns{{</pre>}}*What?!* Well no wonder my tweaks weren't doing anything, all *my* code was taking a scant *35 nanoseconds* to run. The other **99.9%** of the time was spent on that single, stupid call, which also happened to be the one call I couldn't get rid of. However, that isn't the end of the story; _querytime() looks like this:
{{<pre cpp>}}void cHighPrecisionTimer::_querytime(unsigned __int64* _pval)
{
  DWORD procmask=_getaffinity(); 
  HANDLE curthread = GetCurrentThread();
  SetThreadAffinityMask(curthread, 1);
  
  QueryPerformanceCounter((LARGE_INTEGER*)_pval);
  
  SetThreadAffinityMask(curthread, procmask);
}
{{</pre>}}

Years ago, it was standard practice to wrap all calls to QueryPerformanceCounter in a CPU core mask to force it to operate on a single core due to potential glitches in the BIOS messing up your calculations. Microsoft itself had recommended it, and you could find this same code in almost any open-source library that was taking measurements. It turns out that this is [no longer necessary](http://msdn.microsoft.com/en-us/library/windows/desktop/dn553408(v=vs.85).aspx):

{{%blockquote%}}**Do I need to set the thread affinity to a single core to use QPC?**

No. For more info, see Guidance for acquiring time stamps. This scenario is neither necessary nor desirable. {{%/blockquote%}}

I couldn't get rid of the QueryPerformanceCounter call itself, but I could get rid of all that other crap it was doing. I commented it out, and *voilà!* The overhead had been reduced to a scant *340 nanoseconds*, only a tenth of what it had been before. I'm still spending 90% of my calculation time calling that stupid function, but there isn't much I can do about that. Either way, it was a good reminder about the entire reason for using a profiler - bottlenecks tend to crop up in the most unexpected places.

{{<pre>}}BSS Profiler Heat Output: 
[main.cpp:3851] test_PROFILE: 142416 µs   [##########
  [code]: 56575.4 µs   [##########
  [main.cpp:3866] outer: 515.43 ns   [....      
    [code]: 343.465 ns   [...       
    [main.cpp:3868] inner: 171.965 ns   [.         
  [main.cpp:3876] beginend: 173.025 ns   [.         
  [main.cpp:3856] control: 169.954 ns   [.         
{{</pre>}}

I also tried adding standard deviation measurements, but that ended up giving me ludicrous values of 342±27348 ns, which isn't very helpful. Apparently there's quite a lot of variance in function call times, so much so that while the averages always tend to be the same over time, the statistical variance goes through the roof. This is probably why most profilers don't include the standard deviation. I *was* able to add in accurate unprofiled code measurements, though, and the profiler uses a dynamic triple magnitude method of displaying how much time a function takes.
