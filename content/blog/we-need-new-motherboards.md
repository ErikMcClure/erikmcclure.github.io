+++
categories = ["blog"]
comments = []
date = "2022-09-22T16:30:00Z"
title = "We Need New Motherboards Before GPUs Collapse Under Their Own Gravity"
updated = "2022-09-22T16:30:00.000+00:00"

[author]
name = "Erik McClure"

+++

**You can't have a 4-slot GPU. You just can't.**
 
We have finally left sanity behind, with nvidia's 4000 series cards yielding a "clown car" of absurd GPU designs, as [GamersNexus put it](https://www.youtube.com/watch?v=mGARjRBJRX8). These cards are so huge they need "GPU Support Sticks", which are an [actual real thing now](https://www.pcworld.com/article/1072925/custom-nvidia-geforce-rtx-4090-4080s-graphics-cards-insane.html). The fact that we insist on relegating the GPU to interfacing with the system while hanging off of a single, increasingly absurd PCIe 6.0 x16 slot that can [push 128 GBps](https://www.anandtech.com/show/17203/pcie-60-specification-finalized-x16-slots-to-reach-128gbps) is **completely insane**. There is no real ability to just pick the GPU you want and then pair it with a cooler that is *actually attached to the motherboard*. The entire cooling solution has to be in the card itself and we are fast reaching the practical limitations here due to gravity and the laws of physics. Top-heavy GPUs are now essentially giant levers pulling on the PCIe slot, with the only possible anchor point that is above the center of mass being the bracket on one side.

A 4080 series card will demand a whopping 450 W, which dwarfs the Ryzen 9 5900X peak power consumption of only 140 W. That's over 3 times as much power! The graphics card is now drawing more power than the **entire rest of the computer!** We'll have to wait for benchmarks to be sure, but the laws of thermodynamics suggest that the GPU will now also be producing more heat than every other component of the PC, *combined*. And this is the thing we have hanging off of a PCIe slot that doesn't have any other way of mounting a cooling solution to the motherboard?!

**What the FUCK are we doing?!**

Look, I'm not a hardware guy. I just write all the shader code that makes GPUs cry. I don't actually know how we should fix this problem, because I don't know what designs are actually thermally efficient or not. I do know, however, that something has to change. Maybe we can make motherboards with a GPU slot next to the CPU slot and have a unified massive radiator sitting on top of them - or maybe it's a better idea to put the two processor units on opposite ends of the board. I don't know, **just *do* something** so I can use a cooling solution that is actually *screwed into the fucking motherboard* instead of requiring a "GPU Support Stick" so gravity doesn't rip it out of the PCIe slot.

As an example of alternative solutions, [here is an MXM form-factor](https://www.amazon.com/Original-Graphics-Alienware-N14E-GS-A1-Replacement/dp/B081L3KH3T/ref=sr_1_35?keywords=mxm+graphics+card&qid=1663887964&sr=8-35) for laptops that allow them to provide custom cooling solutions appropriate for the laptop.

{{<img src="https://m.media-amazon.com/images/I/71QOqoCJ3WL._AC_SX466_.jpg" width="466" >}}

In fact, the PCIe spec itself actually contains a rear-bracket mount that, if anyone was paying attention, would help address this problem:

{{<img src="/img/pci_standard.png" width="1440" >}}

 See that funky looking metal thing labeled "2" on the diagram? That sure looks like a good alternative to a "support stick" if anyone ever actually paid attention to the spec. Or maybe this second bracket doesn't work very well and we need to rethink how motherboards work entirely. Should we have GPU VRAM slots alongside CPU RAM slots? Is that even possible? Or maybe we can come up with an alternative form factor for GPU cards that you can actually attach to the motherboard with screws?
 
 I have no idea what is or isn't practical, but please, just do something before the GPUs collapse under their own gravity and create strange new forms of matter inside my PC case.