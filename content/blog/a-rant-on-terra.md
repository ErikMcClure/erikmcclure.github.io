+++
categories = ["blog"]
date = ""
draft = true
title = "A Rant On Terra"

+++
Metaprogramming, or the ability to inspect, modify and generate code at compile-time (as opposed to reflection, which is runtime introspection of code), has slowly been gaining momentum. Programmers are finally admitting that, after [accidentally inventing turing complete template systems](https://en.wikipedia.org/wiki/Template_metaprogramming), maybe we should just have proper first-class support for generating code. [Rust has macros](https://doc.rust-lang.org/1.2.0/book/macros.html), Zig has [built-in compile time expressions](https://ziglang.org/documentation/master/#Compile-Time-Expressions), Nim lets you [rewrite the AST](https://nim-lang.org/docs/macros.html) however you please, and [dependent types](https://en.wikipedia.org/wiki/Dependent_type) have been cropping up all over the place. However, with great power comes ~~great responsibility~~ undecidable type systems, whose undefined behavior may involve summoning eldritch abominations from the Black Abyss of Rěgne Ūt.

One particular place where metaprogramming is particularly useful is low-level, high-performance code, which is what [Terra](http://terralang.org/) was created for. The idea behind Terra is that, instead of crafting ancient runes inscribed with infinitely nested variadic templates, just replace the whole thing with an actual turing-complete language, like say, [Lua](https://www.lua.org/) (technically including [LuaJIT](http://luajit.org/) extensions for FFI). This all sounds nice, and no longer requires a circle of salt to ward off demonic syntax. Terra's website is quick to point this out, espousing the magical wonders of replacing your metaprogramming system with an actual scripting language:

> In Terra, we just gave in to the trend of making the meta-language of C/C++ more powerful and replaced it with a real programming language, Lua.
>
> The combination of a low-level language meta-programmed by a high-level scripting language allows many behaviors that are not possible in other systems. Unlike C/C++, Terra code can be JIT-compiled and run interleaved with Lua evaluation, making it easy to write software libraries that depend on runtime code generation.
>
> Features of other languages such as conditional compilation and templating simply fall out of the combination of using Lua to meta-program Terra

Terra even claims you can implement Java-like OOP inheritance models as libraries and drop them into your program. It may also cure cancer, the documentation wasn't clear about that part.

> As shown in the templating example, Terra allows you to define methods on struct types but does not provide any built-in mechanism for inheritance or polymorphism. Instead, normal class systems can be written as libraries. More information is available in our PLDI Paper.
>
> The file lib/javalike.t has one possible implementation of a Java-like class system, while the file lib/golike.t is more similar to Google’s Go language.

I am here to warn you, traveler, that **Terra sits on a throne of lies**. I was foolish. I was taken in by their audacious claims and fake jewels. It is only when I finally sat down to dine with them that I realized I was surrounded by nothing but cheap plastic and slightly burnt toast.

The Bracket Syntax Problem

Terra exists as a syntax extension to Lua. This means it adds additional syntax on top of Lua's existing syntax. Most languages, when extending an existing syntax, would go to great lengths to ensure the new syntax does not create any ambiguities or otherwise interfere with the original syntax, treating it like a delicate flower that mustn't be disturbed, lest it lose a single petal.  
  
Terra takes the flower, gently places it on the ground, and then stomps on it, repeatedly, until the flower is nothing but a pile of rubbish, as dead as the dirt it grew from. Then it sets the remains of the flower on fire, collects the ashes that once knew beauty, drives to a nearby cliffside, and throws them into the uncaring ocean. It probably took a piss too, but I can't prove that.

To understand why, one must understand what the escape operator is. It allows you to splice an abstract AST generated from a lua expression directly into Terra code. Here is an example from Terra's website:
```
function get5()
	return 5
end
terra foobar()
	return [ get5() + 1 ]
end
foobar:printpretty()
> output:
> foobar0 = terra() : {int32}
> 	return 6
> end
```
But, wait, that means it's... the same as the array indexing operator? You don't mean you just put it inside like--
```
local rest = {symbol(int),symbol(int)}

terra doit(first : int, [rest])
    return first + [rest[1]] + [rest[2]]
end
```
What.

**_WHAT?!_**

You were supposed to banish the syntax demons, not summon them! This is an abomination! It is an insult to God's creations, and His ultimate plan. It is the very foundation that Satan himself would use to unleash Evil upon the world. Behold, mortals, for I come as a harbinger of _despair_:
```

```
For those of you joining us (probably because you heard a blood-curdling scream from down the hall), this syntax is exactly as ambiguous as you might think. The most obvious problem is multi-dimensional arrays, because you no longer know if a splice operator is supposed to index the array or act as a splice operator, as detailed in this issue. However, because this is Lua, whose syntax is very much like a delicate flower that cannot be disturbed, there is a much worse ambiguity possible.
```

```
The intent here is shown in the comments. We want to use a spliced lua expression as the array index. However, if no spaces are used, do you know what happens?

It turns into a string, because `[[string]]` is the lua syntax for an unescaped string. Now, those of you who still possess functioning brains may believe that this would result in a syntax error, as we have now placed a string next to a variable. **Not so!** Lua, in it's infinite wisdom, converts anything of the form `symbol"string"` or `symbol[[string]]` into a **function call** with the string as the only parameter. That means we literally attempt to *call our variable as a function with our expression as a string*:
```

```
As a result, you get a *runtime error*, not a syntax error, and a very bizarre one too, because it's going to complain about trying to call a function. This is like trying to bake pancakes for breakfast and accidentally shipping your cat to Abu Dhabi instead. It's not a sequence of events that should ever be related in any universe that obeys causality.





It should be noted that, after a friend of mine heard my screams of agony, [an issue was raised]() to change the syntax to a summoning ritual that involved less self-mutilation. Unfortunately, this is a breaking change, and will probably require an exorcism.

All The Documentation Is Wrong

Terra's documentation is so wrong that it somehow manages to be wrong in both directions. That is, some of the documentation is out-of-date, while some of the documentation refers to concepts that never made it out of the develop branch. I can only assume that a time-traveling gremlin was hired to write the documentation, who promptly got lost admist the diverging timelines. It is a quantum document, both right and wrong at the same time, yet somehow always useless, a puzzle beyond the grasp of modern physics.

The terra symbol operator as a C preprocessor replacement instead of a template

How terra is literally C but the preprocessor is lua

This means that implementing objects is almost impossible because terra has no scoping mechanisms - it's C, so you just implement vtables without being able to do constructors or destructors easily. It does have "defer" but you have to invoke it yourself

If terra was actually trying to build a metaprogramming equivilent to templates, it would have an actual type system. These languages already exist - Idris, etc. etc. etc., but none of them are interested in using their dependent type systems to actually metaprogram low-level code. The problem is that building a recursively metaprogrammable type system requires building a proof assistant, and everyone is so proud of the fact they built a proof assistant they forget that dependent type systems can do other things too, like build really fast memcpy implementations.

In conclusion, everything is terrible and I want to die.