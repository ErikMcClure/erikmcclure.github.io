+++
blogimport = true
categories = ["blog"]
comments = [3147044171597563222, 5484818375220808234, 4505768231957787737, 8136782991325509920]
date = "2012-08-22T18:40:00Z"
title = "What Is A Right Answer?"
updated = "2012-08-22T18:40:19.000+00:00"
draft = true
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I find that modern culture is often obsessed with a concept of *wrongness*. It is a tendency to paint things in a black and white fashion, as if there are simply wrong answers and right answers and nothing in-between. While I have seen this in every single imaginable discipline (including art and music, which is particularly disturbing), it is most obvious to me in the realm of programming.

When people aren't making astonishingly over-generalized statements like trying to say one programming language is better than another without context, we often try to find the "best" way to do something. The problem is that we don't often bother to think about exactly what makes the best answer the *best answer*. Does it have to be fast? If speed was the only thing that was important, we'd write everything in assembly. Does it have to be simple? I could list a thousand instances were simplicity fails to account for edge-cases that render the code useless. Does it have to be easy to understand? If you want something to be easy to understand, then the entire C standard library is one giant wrong answer that's being relied upon by virtually every single program in the entire world.

For a concept taken for granted by most programmers, defining what exactly makes an implementation "optimal" is incredibly difficult. A frightening number of programmers are also incapable of realizing this, and continue to hang on to basic assumptions that one would think should hold everywhere, when very few of them actually do. Things like "the program should not crash" seem reasonable, but what if you want to ensure that a safety feature crashed the program instead of corrupting the system?

The knee-jerk reaction to this is "Oh yeah, *except for that*." This phrase seems to underlie many of the schisms in the programming community. Virtually every single assumption that could be held by a programmer will be wrong ***somewhere***. I regularly encounter programmers who think you should do something a specific way no matter what, until you ask them about making a kernel. "Oh yeah, *except for that*." Or a sound processing library. "Oh yeah, *except for that*." Or a rover on mars. Or a video decoder. Or a raytracer. Or a driver. Or a compiler. Or a robot. Or scientific computing.

All these *except-for-that*'s betray the fundamental failure of modern programming culture: **There is no right answer**. The entire concept of Right and Wrong does not belong in programming, because you are trying to find your way to a solution and there are billions of ways to get there, and the one that works best for your particular situation depends on hundreds of thousands of independent variables. Yet, there is a "right answer" on Stack Overflow. There are books on writing "proper code". There are "best practices". People talk about programming solutions that are "more right" than others. There are black and white, right and wrong, yes or no questions pervading the consciousness of the majority of programmers, who foolishly think that you can actually reduce an engineering problem into a mathematical one, despite [overwhelming evidence](http://www.bailopan.net/blog/?p=7) that you simply cannot escape the clutches of reality and how long it takes an electron to reach the other side of a silicon wafer.

If you ask someone how to do something the right way, you are asking the wrong question. You should be asking them *how to solve your problem*. You didn't do something the wrong way, you simply *solved the wrong problem*.
