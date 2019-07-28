+++
categories = ["blog"]
date = ""
draft = true
title = "RISC Is Fundamentally Unscalable"

+++
Today, there was an announcement about a new RISC-V chip, which has got a lot of people excited. I wish I could also be excited, but to me, this is just a reminder that RISC architectures are fundamentally unscalable, and inevitably stop being RISC as soon as they need to be fast. People still call ARM a "RISC" architecture despite [ARMv8.3-A adding a `FJCVTZS` instruction](https://en.wikipedia.org/wiki/ARM_architecture#ARMv8.3-A), which is "Floating-point Javascript Convert to Signed fixed-point, rounding toward Zero". Reduced instruction set, my ass.

The laws of physics ensure that no RISC architecture can scale under load.