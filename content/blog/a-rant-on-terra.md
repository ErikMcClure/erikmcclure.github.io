+++
categories = ["blog"]
date = ""
draft = true
title = "A Rant On Terra"

+++
Metaprogramming, or the ability to inspect, modify and generate code at compile-time (as opposed to reflection, which is runtime introspection of code), has slowly been gaining momentum. Programmers are finally admitting that, after [accidentally inventing turing complete template systems](https://en.wikipedia.org/wiki/Template_metaprogramming), maybe we should just have proper first-class support for generating code. [Rust has macros](https://doc.rust-lang.org/1.2.0/book/macros.html), Zig has [built-in compile time expressions](https://ziglang.org/documentation/master/#Compile-Time-Expressions), Nim lets you [rewrite the AST](https://nim-lang.org/docs/macros.html) however you please, and [dependent types](https://en.wikipedia.org/wiki/Dependent_type) have been cropping up all over the place. However, with great power comes ~~great responsibility~~ undecidable type systems, whose undefined behavior may involve summoning eldritch abominations from the Black Abyss of Rěgne Ūt.

One particular place where metaprogramming is particularly useful is low-level, high-performance code, which is what [Terra](http://terralang.org/) was created for. The idea behind Terra is that, instead of crafting ancient runes inscribed with infinitely nested variadic templates, just replace the whole thing with an actual turing-complete language, like say, [Lua](https://www.lua.org/) (technically including [LuaJIT](http://luajit.org/) extensions for FFI). This all sounds nice, and no longer requires a circle of salt to ward off demonic syntax, which Terra's website is quick to point out, espousing the magical wonders of replacing your metaprogramming system with an actual scripting language:

> In Terra, we just gave in to the trend of making the meta-language of C/C++ more powerful and replaced it with a real programming language, Lua.
>
> The combination of a low-level language meta-programmed by a high-level scripting language allows many behaviors that are not possible in other systems. Unlike C/C++, Terra code can be JIT-compiled and run interleaved with Lua evaluation, making it easy to write software libraries that depend on runtime code generation.
>
> Features of other languages such as conditional compilation and templating simply fall out of the combination of using Lua to meta-program Terra

Terra even claims you can implement Java-like OOP inheritance models as libraries and drop them into your program. It may also cure cancer, but the documentation wasn't clear.

> As shown in the templating example, Terra allows you to define methods on struct types but does not provide any built-in mechanism for inheritance or polymorphism. Instead, normal class systems can be written as libraries. More information is available in our PLDI Paper.
>
> The file lib/javalike.t has one possible implementation of a Java-like class system, while the file lib/golike.t is more similar to Google’s Go language.

I am here to warn you, traveler, that **Terra sits on a throne of lies**. I was foolish. I was taken in by their audacious claims and fake jewels. It is only when I finally sat down to dine with them that I realized I was surrounded by nothing but cheap plastic and fed slightly burnt toast.

The \[\] terra syntax problem

But, wait, that means it's... the same as the array indexing operator? You don't mean you just put it inside like--

What.

WHAT?!

You were supposed to banish the syntax demons, not summon them!

It should be noted that, after a friend of mine heard my screams of agony, [an issue was raised]() to change the syntax to something that involved less self-mutilation. Unfortunately, this is a breaking change, and as a result will probably require performing an exorcism.

The terra symbol operator as a C preprocessor replacement instead of a template

How terra is literally C but the preprocessor is lua

This means that implementing objects is almost impossible because terra has no scoping mechanisms - it's C, so you just implement vtables without being able to do constructors or destructors easily. It does have "defer" but you have to invoke it yourself

If terra was actually trying to build a metaprogramming equivilent to templates, it would have an actual type system. These languages already exist - Idris, etc. etc. etc., but none of them are interested in using their dependent type systems to actually metaprogram low-level code. The problem is that building a recursively metaprogrammable type system requires building a proof assistant, and everyone is so proud of the fact they built a proof assistant they forget that dependent type systems can do other things too, like build really fast memcpy implementations.

In conclusion, everything is terrible and I want to die.