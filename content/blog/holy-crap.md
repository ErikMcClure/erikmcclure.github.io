+++
title = "Holy crap"
date = 2010-02-12T01:00:00Z
updated = 2011-01-22T04:17:38Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

I must have accidentally imbued my to-do-list with rabbits or something, because it just gets bigger and bigger.

**To-Do**
- _bucketsort cRBT_list isn't working because the getnear function doesn't differentiate between a "before" and an "after."
 ○ Implement checking for this so it can fail appropariately
 ○ Figure out if it needs to be a before or an after for the actual getnear call in the alloc function
- Do stress tests on the memory allocator and make sure it doesn't fail under any circumstances
- Do a test on the string pool allocator being inside the DLL and try to figure out why the hell the allocator gets initialized like 6 times.
- Finalize the string pool for usage
- Finish the last bit of the string table

- Build kd-tree implementation.
 ○ Each layer swaps between x and y and has a value that specifies where to make the split.
 ○ A balance integer is required so the tree can keep itself roughly balanced, but this should be athreshold difference of about 5 or so to prevent a single object from constantly resizing the tree.
 ○ Each layer holds a list of renderables of that depth. A renderable's total radius must fit entirely inside the branch in question, or if it has no rotation, do a simple bounds check.
 ○ Most of the work is done when a renderable is inserted, because this is when the bounds checks get made
 ○ When a renderable moves, it just checks its nearby nodes for a needed crossover.
 ○ When a renderable changes dimension, it may need to be moved to a higher level.
  § Note that you may be able to combine the scale and movement checks into a single bounds check
- Remove the radial check from anything using the kd-tree, but cImageZ doesn't use the kd-tree so keep the radial check for that one.
- Because use of the kd-tree is optional, you need a way to standardize its use. Some kind of function somewhere saying "Add to render queue" or "add to kd-tree" or something.
- Swap the render buffer to use an additive memory allocator
 ○ You will need to create a separate render buffer to maintain a list of renderables that don't use the kd-tree. 
 ○ To do this properly you'll want to create a tree merge function with a mass allocation, which will allow for the transfer of memory in one batch per-frame, which should be crazy fast.
 ○ Do this first, then start adding in stuff from the kd-tree.
- Do stress testing on the kd-tree
 ○ Make line renders for the tree (that'll be a lot of fun to watch)
 ○ Ensure optimal performance on low and high density images.
  § Also check the resulting performance hit on a single image

Implement front-to-back transparency blending
- First, undo all that stuff you just did.
- The first thing that needs to be working is the multi-texture technique.
- Then, you need to make sure the blending is there, and that the backbuffer has an alpha (Should be done already)
- At the end of the render, reset the alpha channel to opaque
- Now you should be able to see the results. Ensure the blending functions properly under high tranparency complexities and adjust it as necessary
- Once the blending is working perfectly, enable the stencil buffer and do a write when the pixel is opaque (or in the case of this algorithm, completely transparent.
- Now extend this to multi-texture scenarios
- Then, ensure that it still works with the lighting technique.
 ○ Later on you can figure out if the alternate lighting method is still viable
- Done properly, the normalmaps should automatically use this technique as well. You may need to tweak that a bit though

- Go back to implementing the lighting system
 ○ Ensure blending functions correctly
 ○ Implement shadows (No penumbra! but still use the same circle calculation so your getting the correct umbra)
 ○ Calculate soft shadow points
 ○  Build soft shadow triangles
 ○  put in option to have object either be affected by light or not (Do it as a flag)
 ○  Put in coronas
 ○  put in option to have light cast shadows instead of be occluded. This is used for things like the sun, where color is ignored.
 ○  Optimize
 ○  Implement arc culling
 ○  ensure rect culling is working
 ○  Ensure backup textures allow it to work on the laptop

- Implement new text renderer
- Do C# interop
