+++
title = "Assembly CAS implementation"
date = 2010-07-22T12:08:00Z
updated = 2011-01-22T04:05:49Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

{{<pre cpp>}}inline unsigned char BSS_FASTCALL asmcas(int *pval, int newval, int oldval)
  {
      unsigned char rval;
      __asm {
#ifdef BSS_NO_FASTCALL //if we are using fastcall we don't need these instructions
        mov EDX, newval
        mov ECX, pval
#endif
        mov EAX, oldval
        lock cmpxchg [ECX], EDX
        sete rval // Note that sete sets a 'byte' not the word
      }
      return rval;
  }{{</pre>}}
This was an absolute bitch to get working in VC++, so maybe this will be useful to someone, somewhere, somehow. The GCC version I based this off of can be found [here](http://pages.cs.wisc.edu/~remzi/Classes/537/Fall2005/Projects/P3/cas.c).

Note that, obviously, this will only work on x86 architecture.
