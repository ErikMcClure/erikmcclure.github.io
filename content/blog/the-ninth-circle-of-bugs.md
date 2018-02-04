+++
title = "The Ninth Circle of Bugs"
date = 2011-05-15T04:43:00Z
updated = 2011-05-15T04:43:28Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

So I'm rewriting my [2D culling kd-tree](www.blackspherestudios.com/storage/McClure_Erik_Regional_KD_Trees.pdf) for my graphics engine, and a strange bug pops up. On release mode, one of the images vanished. Since it didn't happen in debug mode, it was already a {{<code>}}heisenbug{{</code>}}. A {{<code>}}heisenbug{{</code>}} is defined as a bug that vanishes when you try to find it. It took me almost a day to trace the bug to the {{<code>}}rebalance{{</code>}} function. At first I thought the image had simply been removed from a node accidentally, but this wasn't the case. It took another day to finally figure out that the {{<code>}}numimages{{</code>}} variable was getting set to 0, thus causing the node to think it was empty and resulted in it deleting itself and removing itself from the tree (which caused all sorts of other problems).

Unfortunately, I could not verify the tree. Any attempt that so much as *touched* the tree's memory would wipe out the bug, or so I thought. Then I tried adding the verification function into an if statement that would only activate if the bug appeared - it did not. The act of adding a line of code *that was never executed* actually **caused the bug to vanish**.

I was absolutely stunned. This was completely insane. Following the advice of a friend, I was forced to assume the compiler somehow screwed something up, so I randomly disabled various optimizations in release mode. It turned out that disabling the {{<code>}}Omit Frame Pointers{{</code>}} optimization removed the bug. I didn't actually know what frame pointers *were*, only that I had turned on this optimization in many other projects for the hell of it and it had never caused any problems (no optimizations ever should, for that matter). What I discovered [was astonishing](http://www.nynaeve.net/?p=91). Frame pointers couldn't be omitted from a function if it got too complicated or needed to unwind the stack due to a possible exception. On a hunch, instead of adding the verification function to the chunk of code that was only executed if the error occurred, I instead added a vestigial {{<code>}}'throw "derp";'{{</code>}} line.

*The problem vanished*.

I knew instantly that either the problem was caused by the omission of frame pointers, which would indicate a bug in the VC++ 2010 compiler (unlikely), or when the frame pointers were included, it masked the bug (much more likely). But I also had another piece of knowledge at my disposal - exactly how I could modify the function without masking the bug. I considered decompiling the function and forcing VC++ to use the flawed assembly, but that didn't allow me to modify the assembly in any meaningful way. A bit more experimentation revealed that any access of the root node, or for that matter, the {{<code>}}'this'{{</code>}} pointer itself (unless it was for calling a function) caused the inclusion of the frame pointer. I realized that a global variable would be exempt from this, and that I might be able to get by this limitation by assigning the address of whatever variable I needed to the global variable and passing that into the function instead.

This approach, however, failed. In fact most attempts to get around the frame pointer inclusion failed. I did, however, notice what appeared to be a separate bug in another part of the tree. A short investigation later revealed an unrelated bug in the tree caused by the solve function. However, what was causing this bug (duplicated parentC pointers) still threw up errors after solving the first bug, indicating that it was possible this mysterious insane compiler induced bug was just a symptom of a deeper one that would be easier to detect. After more hunting, a second unrelated bug was found. Clearly this tree was not nearly as stable as I had thought it was.

A third bug was eventually found, and I discovered the root cause of this bug to be an #NaN float value in the tree. This should never ever, ever happen, because it destabilizes the tree, but sure enough, I finally found the cause.

{{<code>}}_totalremove(node->total,(const float (&)[4])currect);{{</code>}}

Casting from a float* that was previous cast from a float[4] causes read errors at totally random times, despite this being completely valid under the circumstances. My only guess is that the compiler somehow interpreted this cast as undefined behavior and went crazy. I will never know. All I know is that I should never, ever, ever, ever, ever cast to that data type ever again, because guess what? After removing all my debug equipement and putting the cast back in, I was able to reliable reproduce the bug that started this whole mess, and removing the cast made the bug vanish.

This entire week long trek through hell was because the compiler fucked up on a goddamn *variable cast*. It wasn't a memory leak, it wasn't a buffer overrun, it was just a goddamn *miscast variable*.

Lesson: Re-validate every inch of your data structure the instant you realize you have a heisenbug, and make sure your validation function properly checks for all things that can screw things up.
