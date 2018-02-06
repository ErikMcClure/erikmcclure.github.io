+++
blogimport = true
categories = ["blog"]
date = "2011-04-10T17:06:00Z"
title = "Investigating Low-level CPU Performance"
updated = "2011-04-10T17:18:39.000+00:00"
comments = [ 845834464275820721, 4602207182752990567, 4044932015689581430, 1267508186017457128 ]
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
While reconstructing my threaded Red-Black tree data structure, I naturally assumed that due to invalid branch predictions costing significant amounts of performance, by eliminating branching in low-level data structures, one can significant enhance the performance of your application. I did some profiling and was stunned to discover that my new, optimized Red Black tree was... *SLOWER* then the old one! This can't be right, I eliminated several branches and streamlined the whole thing, how can it be *SLOWER?!* I tested again, and again, and again, but the results were clear - even with fluctuations of up to 5% in the results, the average speed for my new tree was roughly 7.5% larger then my old one (the following numbers are the average of 5 tests).

Old: 626699 ticks
New: 674000 ticks

{{<pre>}}//Old
c = C(key, y->_key);
if(c==0) return y;
if(c<0) y=y->_left;
else y=y->_right;

//New
if(!(c=C(key,y->_key)))
return y;
else
y=y->_children[(++c)>>1];
{{</pre>}}

Now, those of you familiar with CPU branching and other low-level optimizations might point out that the compiler may have optimized the old code path more effectively, leaving the new code path with extra instructions due to the extra increment and bitshift operations. **Wrong.** Both code paths have the *exact same number of instructions*. Furthermore, there are only ***FOUR*** instructions that are different between the two implementations (highlighted in red below).

{{<span>}}
<table cellpadding="0" cellspacing="0"><tr><td><b>New</b>
<pre>00F72DE5  mov         esi,dword ptr [esp+38h]  
00F72DE9  mov         eax,dword ptr 
00F72DEE  cmp         esi,eax  
00F72DF0  je          main+315h (0F72E25h)  
00F72DF2  mov         edx,dword ptr [esp+ebx*4+4ECh]
<span style="color:#FF0000;">00F72DF9  lea         esp,[esp]</span>
00F72E00  mov         edi,dword ptr [esi+4]  
00F72E03  cmp         edx,edi  
00F72E05  jge         main+2FCh (0F72E0Ch)  
00F72E07  or          ecx,0FFFFFFFFh  
00F72E0A  jmp         main+303h (0F72E13h)  
00F72E0C  xor         ecx,ecx  
00F72E0E  cmp         edx,edi  
00F72E10  setne       cl  
00F72E13  movsx       ecx,cl  
00F72E16  test        ecx,ecx  
00F72E18  je          main+317h (0F72E27h)  
<span style="color:#FF0000;">00F72E1A  inc         ecx  </span>
<span style="color:#FF0000;">00F72E1B  sar         ecx,1  </span>

00F72E1D  mov         esi,dword ptr [esi+ecx*4+18h]  
00F72E21  cmp         esi,eax  
00F72E23  jne         main+2F0h (0F72E00h)  
00F72E25  xor         esi,esi  
00F72E27  mov         eax,dword ptr [esi]  
00F72E29  add         dword ptr [esp+1Ch],eax</pre>
</td><td><b>Old</b>
<pre>00F32DF0  mov         edi,dword ptr [esp+38h]  
00F32DF4  mov         ebx,dword ptr 
00F32DFA  cmp         edi,ebx  
00F32DFC  je          main+31Dh (0F32E2Dh)  
00F32DFE  mov         edx,dword ptr [esp+eax*4+4ECh]  

00F32E05  mov         esi,dword ptr [edi+4]  
00F32E08  cmp         edx,esi  
00F32E0A  jge         main+301h (0F32E11h)  
00F32E0C  or          ecx,0FFFFFFFFh  
00F32E0F  jmp         main+308h (0F32E18h)  
00F32E11  xor         ecx,ecx  
00F32E13  cmp         edx,esi  
00F32E15  setne       cl  
00F32E18  movsx       ecx,cl  
00F32E1B  test        ecx,ecx  
00F32E1D  je          main+31Fh (0F32E2Fh)  
<span style="color:#FF0000;">00F32E1F  jns         main+316h (0F32E26h)  </span>
<span style="color:#FF0000;">00F32E21  mov         edi,dword ptr [edi+18h]  </span>
<span style="color:#FF0000;">00F32E24  jmp         main+319h (0F32E29h)  </span>
00F32E26  mov         edi,dword ptr [edi+1Ch]  
00F32E29  cmp         edi,ebx  
00F32E2B  jne         main+2F5h (0F32E05h)  
00F32E2D  xor         edi,edi  
00F32E2F  mov         ecx,dword ptr [edi]  
00F32E31  add         dword ptr [esp+1Ch],ecx</pre>
</td></tr></table>{{</span>}}

I have no real explanation for this behavior, but I do have a hypothesis: The important instruction is the extra *LEA* in my new method that appears to be before the branch itself. As a result, it may be possible for the CPU to be doing branch prediction in such a way it shaves off one instruction, which gives it a significant advantage. It may also be that the branching is just *faster* then my increment and bitshift, although I find this highly unlikely. At this point I was wondering if anything I knew about optimization held any meaning in the real world, or if everything was just a lot of guesswork and profiling because *what the fuck?!* However, it then occurred to me that there was an optimization possible for the old version - Move the if(c==0) statement to the bottom so the CPU does the (c<0) and (c>0) comparisons first, since the c==0 comparison only happens once in the traversal. Naturally I was a bit skeptical of this having any effect on the assembly-rewriting, branch-predicting, impulsive teenage bitch that my CPU was at this point, but I tried it anyway.

It worked. There was a small but noticeable improvement in running time by using the old technique and rewriting the if statements as such:
{{<pre>}}c = C(key, y->_key);
if (c < 0)  y = y->_left;
else if(c > 0) y = y->_right;
else return y;
{{</pre>}}

Optimized: 610161.8 Ticks

The total performance improvement over my failed optimization attempt and my more successful branch-manipulation technique is a whopping 63838.2 Ticks, or a ~10% improvement in speed, caused by simply rearranging 4 or 5 instructions. These tests were done on a randomized collection of 500000 integers, so that means the optimized version can pack in 10% more comparisons in the same period of time as the bad optimization. That's 550000 vs 500000 elements, which seems to suggest that delicate optimization, even in modern CPUs, can have significant speed improvements. Those of you who say that toying around with low level code can't infer significant performance increases should probably reconsider exactly what you're claiming. This wouldn't directly translate to 50000 extra players on your server, but a 10% increase in speed isn't statistically insignificant.
