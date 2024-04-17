+++
categories = ["blog"]
comments = []
date = "2021-10-18T01:06:00Z"
title = "C Programmers Are Sabotaging Computer Science"
updated = "2021-10-18T01:06:00.000+00:00"
draft = true
[author]
name = "Erik McClure"

+++

Recently, the White House [put out a statement]() asking programmers to move towards memory-safe languages. This has predictably resulted in a lot of angry C programmers, which is undeniably very funny. What is less funny are C programmers writing *incredibly bad articles* that give terrible advice and are dangerously wrong. I was recently linked to one article that is ***so bad*** I feel obligated to post a rebuttal as a public service. 

In [C skill issue; how the White House is wrong](https://felipec.wordpress.com/2024/03/03/c-skill-issue-how-the-white-house-is-wrong/), the author begins with a very long and confusing analogy that tries to explain how some C programmers are *way better* that "average" C programmers, and their superhuman skills allow them to actually write memory-safe code. I'm not going to contest this, because nothing I write could possibly do a better job of falsifying this claim than the article itself.

## Exhibit 1

We start with nonsensical C code that is hopefully constructed for demonstration purposes only:

{{pre




{{%blockquote%}}
When a function is called, there’s a place in memory for all the information related to this function, and that includes local variables. When the function is exited, that memory is repurposed.

This is not even a feature of C, this is a feature of the CPU. C being a low-level language makes these CPU features transparent.
{{%/blockquote%}}

Almost every part of this is wrong. There is no magical place in memory for "information related to this function", it's just the stack, and in C, there is only one of it. The author had a great opportunity here to explain the difference between the stack and the heap, but never actually does, which means he also never explains how it's possible for stack corruption to happen. Then he proceeds to say the most insane thing I've ever heard a low-level programmer say: "this is a feature of the CPU".

*No!*

***NO!***

The CPU has *no idea* what your program is doing. It doesn't even know what a stack is. All it does is [provide some registers](https://stackoverflow.com/questions/21718397/what-are-the-esp-and-the-ebp-registers) you *could* use to store the current frame pointer (EBP/RBP) and the current stack pointer (ESP/RSP). Using these registers is completely optional, and C code only actually needs the frame pointer when your stack size is dynamic. Most of the time, the amount of stuff allocated on the stack is known at compile time, so some compilers [include an optimization flag](https://learn.microsoft.com/en-us/cpp/build/reference/oy-frame-pointer-omission?view=msvc-170) that avoids using the frame pointer register when it isn't necessary, letting it be used for other things. Now, you might ask when the stack size *isn't* known at compile time, which is a very interesting question! This would have been easier to explain if we had *explained how the stack worked*, but there are basically two cases where stack size is unknowable: [`alloca`](https://man7.org/linux/man-pages/man3/alloca.3.html) was used in a function call above the current function, or [Variable-Length Arrays](https://en.cppreference.com/w/c/language/array#Variable-length_arrays) are being used. `alloca` lets you allocate an arbitrary amount of memory on the stack (but risks a stack overflow if left unguarded), and can be used to, among other things, construct a temporary linked list on the callstack itself, which is useful in some very esoteric cases. Some other edge cases may arise depending on hardware, toolchain, compiler extensions, etc, but for the most part, if you don't use either `alloca` or VLAs, you don't need a frame pointer.

Furthermore, all this applies to *almost all programming languages*, because most of them will at least store _primitive_ values on the stack, even if they hide this from programmers. Even then, managed languages like C# have an explicit facility for allocating things on the stack instead of the heap, like [types with value semantics](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/struct), and [Java will have them soon-ish](https://openjdk.org/projects/valhalla/design-notes/state-of-valhalla/01-background). C being a low-level language *isn't relevent here!* You store things on the stack in lots of languages, just with different semantics! In fact, some languages have *multiple stacks* (like [Go](https://blog.cloudflare.com/how-stacks-are-handled-in-go)) and this has important consequences for their code generation! 

Not only is managing the function state not a feature of the CPU, but how it happens matters in C because of calling conventions. Saying hand-wavey things like "memory is repurposed" hides the fact that the function usually needs to store the previous state of some (or all) of the registers and then restore them before returning. Who does this storing and restoring and which registers need to be preserved and which values are allowed to be passed by register and what order they are passed in are all part of the calling convention. This used to be extremely important to know about when compiling against the 32-bit WIN32 API, because it used `__stdcall` instead of `__cdecl`, which swapped who stores and restores the register values (). Microsoft switched to the standard x64 calling convention when moving to 64-bit, but different calling conventions still exist, like `__vectorcall` that tries to pass floats through SSE registers. This hand-waving also means we skip over how the frame pointer is popped on to the stack pointer, if it was previously stored, or a known constant is subtracted from the current stack pointer (which is how the compiler can elide storing the frame pointer).







Telling people to make variables `static` to avoid copying them on to the stack repeatedly is utterly insane advice. 




This struct was supposed to be a *constant*. The reason he would put it in a static to try to avoid "copying the same data over and over" is because... it's constant. *But he hasn't used the `const` keyword*, or ***even mentioned it!!!*** Our "Ace C Programmer" has left out *the most important semantic information* that good code should actually have: whether or not the variable is intended to be mutable or not. Now, some people may point out that C ignores the `const` keyword, and we can confirm [it doesn't change the assembly output](), except when you are using a `static` variable this is **the one place it can actually matter**. The thing is, when you're writing C code for an *embedded device*, the compiler will start doing weird stuff, and if you say something is a constant static, certain C compilers on certain toolchains will compile that variable to ROM instead of RAM. That means if you cast away the `const`, you're *definitely* going to cause an access violation. So this is *THE ONLY SITUATION* where using `const` actually matters, and they didn't even mention it.

As a result, this "advice" is maximally harmful, because at no point does the author describe the gigantic, massive footgun that lurks behind *mutable* static variables: they aren't threadsafe. Heck, even if you make them a thread-local static to try get around this, you run into the famous `strtok` problem. [Everything tells you]() to never use `strtok` and to instead use something like [`strtok_s`]() or [`strtok_r`](), because `strtok` stores the current state of the string search in a static variable. This means that *even in a single-threaded program*, you can **NEVER** nest calls to `strtok`, which is extremely easy to do by accident if you are, for example, parsing something recursively, because the second recursive strtok will obliterate the state of the first strtok and then everything will explode. It's not enough to call this "not threadsafe", it's "not [re-entrant](https://en.wikipedia.org/wiki/Reentrancy_(computing)) safe", in that it's not safe to use in any recursive context because it hasn't actually finished executing until you've gone through the entire string.

The author discusses none of this, instead ending our first example with the extremely ironic "Let’s keep in mind that this is the absolute most basic example I could think of, and already the difference between a beginner and an expert is substantial."

The difference is very substantial, indeed. But lets keep going.



## Exhibit 2







The entire point of this article is to demonstrate good C code ("In this post I only scratched the surface of what top C code looks like") yet all they have demonstrated is that ***they do not understand how a computer works***. What's even more infuriating is that they talk about *using a disassembler* later on, which they could have used to examine how the calling convention and stack handling was done!

{{%blockquote%}}
comparing the assembly code generated by different C codes is not overkill: it’s something C experts do regularly, with different compiler options, and even different compilers. One single line of C code can have a tremendous amount of work and thought behind.
{{%/blockquote%}}
 What do you MEAN it would be too cumbersome to write out the assembly code?! HERE IT IS:
 
 
 
 
 






> *Disclaimer: This is a rant, not some well-researched article trying to make a point. I don't have a point. I'm just really mad.*

Before we start, if you use C out of *necessity*, this article is not about you. If you're using it because you're either on a really weird platform, doing embedded work, or otherwise don't really have much choice, then you have a *reason* to be using C, which is fine. What I want to talk about are the absolute *maniacs* who could use something else, but continue to use C in [current year] instead of C++, Rust, Zig, or really just anything else.

Years ago, I once asked a Javascript programmer why they weren't interested in statically typed languages and they said it was because compiling was slow. While we know iteration time is important for productivity, I really don't want Javascript in my fucking car.