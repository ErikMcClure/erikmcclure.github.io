+++
title = "Function Pointer Speed"
date = 2010-07-13T22:24:00Z
updated = 2011-01-22T04:07:39Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

So after a lot of misguided profiling where I ended up just testing the stupid CPU cache and its ability to fucking predict what my code is going to do, I have, for the most part, demonstrated the following:

{{<code>}}if(!(rand()%2d)) footest.nothing();
else footest.nothing2();{{</code>}}

is slightly faster then

{{<code>}}(footest.*funcptr[rand()%2])();{{</code>}}

where funcptr is an array of the possible function calls. I had suspected this after I looked at the assembly, and a basic function pointer call like that takes around 11 instructions whereas a normal function call takes a single instruction.

In debug mode, however, if you have more then 2 possibilities, a switch statement's very existence takes up almost as many instructions as a single function pointer call, so the function pointer array technique is significantly faster with 3 or more possibilities. However, in release mode, if you write something like switch(rand()%3) and then just the function calls, the whole damn thing gets its own super special optimization that reduces it to about 3 instructions and hence makes the switch statement method slightly faster.

In all of these cases though the speed difference for 1000 calls is about 0.007 milliseconds and varies wildly. The CPU is doing so much architectural optimization that it most likely doesn't really matter which method is used. I do find it interesting that the switch statement gets super-optimized in certain situations, though.
