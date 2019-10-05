+++
categories = ["blog"]
date = "2019-10-04T17:08:00Z"
draft = false
title = "A Rant On Terra"

+++
Metaprogramming, or the ability to inspect, modify and generate code at compile-time (as opposed to reflection, which is runtime introspection of code), has slowly been gaining momentum. Programmers are finally admitting that, after [accidentally inventing turing complete template systems](https://en.wikipedia.org/wiki/Template_metaprogramming), maybe we should just have proper first-class support for generating code. [Rust has macros](https://doc.rust-lang.org/1.2.0/book/macros.html), Zig has [built-in compile time expressions](https://ziglang.org/documentation/master/#Compile-Time-Expressions), Nim lets you [rewrite the AST](https://nim-lang.org/docs/macros.html) however you please, and [dependent types](https://en.wikipedia.org/wiki/Dependent_type) have been cropping up all over the place. However, with great power comes ~~great responsibility~~ undecidable type systems, whose undefined behavior may involve summoning eldritch abominations from the Black Abyss of Rěgne Ūt.

One particular place where metaprogramming is particularly useful is low-level, high-performance code, which is what [Terra](http://terralang.org/) was created for. The idea behind Terra is that, instead of crafting ancient runes inscribed with infinitely nested variadic templates, just replace the whole thing with an actual turing-complete language, like say, [Lua](https://www.lua.org/) (technically including [LuaJIT](http://luajit.org/) extensions for FFI). This all sounds nice, and no longer requires a circle of salt to ward off demonic syntax, which Terra is quick to point out. They espouse the magical wonders of replacing your metaprogramming system with an actual scripting language:

> In Terra, we just gave in to the trend of making the meta-language of C/C++ more powerful and replaced it with a real programming language, Lua.
>
> The combination of a low-level language meta-programmed by a high-level scripting language allows many behaviors that are not possible in other systems. Unlike C/C++, Terra code can be JIT-compiled and run interleaved with Lua evaluation, making it easy to write software libraries that depend on runtime code generation.
>
> Features of other languages such as conditional compilation and templating simply fall out of the combination of using Lua to meta-program Terra

Terra even claims you can implement Java-like OOP inheritance models as libraries and drop them into your program. It may also cure cancer (the instructions were unclear).

> As shown in the templating example, Terra allows you to define methods on struct types but does not provide any built-in mechanism for inheritance or polymorphism. Instead, normal class systems can be written as libraries. More information is available in our PLDI Paper.
>
> The file lib/javalike.t has one possible implementation of a Java-like class system, while the file lib/golike.t is more similar to Google’s Go language.

I am here to warn you, traveler, that **Terra sits on a throne of lies**. I was foolish. I was taken in by their audacious claims and fake jewels. It is only when I finally sat down to dine with them that I realized I was surrounded by nothing but cheap plastic and slightly burnt toast.

## The Bracket Syntax Problem

Terra exists as a syntax extension to Lua. This means it adds additional keywords on top of Lua's existing grammar. Most languages, when extending a syntax, would go to great lengths to ensure the new grammar does not create any ambiguities or otherwise interfere with the original syntax, treating it like a delicate flower that mustn't be disturbed, lest it lose a single petal.

Terra takes the flower, gently places it on the ground, and then stomps on it, repeatedly, until the flower is nothing but a pile of rubbish, as dead as the dirt it grew from. Then it sets the remains of the flower on fire, collects the ashes that once knew beauty, drives to a nearby cliffside, and throws them into the uncaring ocean. It probably took a piss too, but I can't prove that.

To understand why, one must understand what the escape operator is. It allows you to splice an abstract AST generated from a Lua expression directly into Terra code. Here is an example from Terra's website:
{{<pre lua>}}function get5()
  return 5
end
terra foobar()
  return [ get5() + 1 ]
end
foobar:printpretty()
> output:
> foobar0 = terra() : {int32}
> 	return 6
> end{{</pre>}}
But, wait, that means it's... the same as the array indexing operator? You don't mean you just put it inside like--

{{<pre lua>}}local rest = {symbol(int),symbol(int)}

terra doit(first : int, [rest])
  return first + [rest[1]] + [rest[2]]
end{{</pre>}}

What.

**_WHAT?!_**

You were supposed to banish the syntax demons, not join them! This _abomination_ is an insult to Nine Kingdoms of Asgard! It is the very foundation that Satan himself would use to unleash Evil upon the world. Behold, mortals, for I come as the harbinger of _despair_:

{{<pre lua>}}function idx(x) return `x end
function gen(a, b) return `array(a, b) end

terra test()
  -- Intended to evaluate to array(1, 2) 0
  return [gen(1, 2)][idx(0)]
end{{</pre>}}

For those of you joining us (probably because you heard a blood-curdling scream from down the hall), this syntax is exactly as ambiguous as you might think. Is it two splice statements put next to each other, or is a splice statement with an array index? You no longer know if a splice operator is supposed to index the array or act as a splice operator, as [mentioned in this issue](https://github.com/StanfordLegion/legion/issues/522). Terra "resolves this" by just assuming that any two bracketed expressions put next to each other are _always_ an array indexing operation, which is a lot like fixing your server overheating issue by running the fire suppression system all day. However, because this is Lua, whose syntax is very much like a delicate flower that cannot be disturbed, a much worse ambiguity comes up when we try to fix this.

{{<pre lua>}}function idx(x) return `x end
function gen(a, b) return `array(a, b) end

terra test()
  -- This is required to make it evaluate to array(1,2)[0]
  -- return [gen(1, 2)][ [idx(0)] ]
  -- This doesn't work:
  return [gen(1, 2)][[idx(0)]]
  -- This is equivalent to:
  -- return [gen(1, 2)] "idx(0)"
end{{</pre>}}

We want to use a spliced Lua expression as the array index, but if we don't use any spaces, _it turns into a string_ because `[[string]]` is the Lua syntax for an unescaped string! Now, those of you who still possess functioning brains may believe that this would always result in a syntax error, as we have now placed a string next to a variable. **Not so!** Lua, in it's infinite wisdom, converts anything of the form `symbol"string"` or `symbol[[string]]` into a **function call** with the string as the only parameter. That means that, in certain circumstances, we literally attempt to _call our variable as a function with our expression as a string_:

{{<pre lua>}}local lookups = {x = 0, y = 1, z = 2, w = 3 };
  vec.metamethods.__entrymissing = macro(function(entryname, expr)
    if lookups[entryname] then
      -- This doesn't work
      return `expr.v[[lookups[entryname]]]
      -- This is equivalent to
      -- return `expr.v "lookups[entryname]"
      -- But it doesn't result in a syntax error, becase it's equivalent to:
      -- return `extr.v("lookups[entryname]")
    else
      error "That is not a valid field."
    end
  end){{</pre>}}

As a result, you get a _type error_, not a syntax error, and a very bizarre one too, because it's going to complain that `v` isn't a function. This is like trying to bake pancakes for breakfast and accidentally going scuba diving instead. It's not a sequence of events that should ever be related in any universe that obeys causality.

It should be noted that, after a friend of mine heard my screams of agony, [an issue was raised](https://github.com/zdevito/terra/issues/385) to change the syntax to a summoning ritual that involves less self-mutilation. Unfortunately, this is a breaking change, and will probably require an exorcism.

## The Documentation Is Wrong

Terra's documentation is so wrong that it somehow manages to be wrong in both directions. That is, some of the documentation is out-of-date, while some of it refers to concepts that never made it into `master`. I can only assume that a time-traveling gremlin was hired to write the documentation, who promptly got lost amidst the diverging timelines. It is a quantum document, both right and wrong at the same time, yet somehow always useless, a puzzle beyond the grasp of modern physics.

* The first thing talked about in the [API Reference](http://terralang.org/api.html#lua-api) is a List object. It does not actually exist. A primitive incarnation of it does exist , but it only implements `map()` Almost the entire section is completely wrong for the `1.0.0-beta1` release. The actual List object being described [sits alone and forgotten](https://github.com/zdevito/terra/blob/develop/src/terralist.lua) in the develop branch, dust already beginning to collect on it's API calls, despite those API calls being the ones in the documentation... somehow.
* `:printpretty()` is a function that prints out a pretty string representation of a given piece of Terra code, by parsing the AST representation. On it's face, it does do exactly what is advertised: it prints a string. However, one might assume that it _returns_ the string, or otherwise allows you to do something with it. This doesn't happen. It literally calls the `print()` function, throwing the string out the window and straight into the `stdout` buffer without a care in the world. If you want the _actual string_, you must call either `layoutstring()` (for types) or `prettystring()` (for quotes). Neither function is documented, anywhere.
* Macros can only be called from inside Terra code. Unless you give the constructor two parameters, where the second parameter is a function called from inside a Lua context. This behavior is not mentioned in any documentation. anywhere, which makes it even more confusing when someone defines a macro as `macro(myfunction, myfunction)` and then calls it from a Lua context, which, according to the documentation, should be impossible.
* Struct fields are not specified by their name, but rather just held in a numbered list of {name, type} pairs. This _is_ documented, but a consequence of this system is not: Struct field names do not have to be unique. They can all be the same thing. Terra doesn't actually care. You can't actually be sure that any given field name lookup will result in, y'know, _one field_. Nothing mentions this.
* The documentation for saveobj is a special kind of infuriating, because everything is _technically_ correct, yet it does not give you any examples and instead simply lists a function with 2 arguments and 4 interwoven optional arguments. In reality it's absolutely trivial to use because you can ignore almost all the parameters. Just write `terralib.saveobj("blah", {main = main})` and you're done. But there isn't a _single example of this_ anywhere on the entire website. Only a paragraph and two sentences explaining in the briefest way possible how to use the function, followed by a highly technical example of how to initialize a custom target parameter, which _doesn't actually compile_ because it has errant semicolons. This is literally _the most important function_ in the entire language, because it's what actually compiles an executable!
* The `defer` keyword is critical to being able to do proper error cleanup, because it functions similar to Go's defer by performing a function call at the end of a lexical scope. It is not documented, anywhere, or even mentioned at all on the website. How Terra manages to implement new functionality it forgets to document while, _at the same time_, documenting functionality _that doesn't exist yet_  is a 4-dimensional puzzle fit for an extra-dimensional hyperintelligent race of aliens particularly fond of BDSM.
* You'd think that compiling Terra on Linux would be a lot simpler, but [you'd be wrong](https://github.com/zdevito/terra/issues/393). Not only are the makefiles unreliable, but cmake itself doesn't seem to work with LLVM 7 unless you pass in [a very specific set of flags](https://github.com/zdevito/terra/blob/master/.travis.yml#L22), none of which are documented, because compiling via cmake **isn't documented at all**, and this is the only way to compile with LLVM 7 or on the latest Ubuntu release!

Perhaps there are more tragedies hidden inside this baleful document, but I cannot know, as I have yet to unearth the true depths of the madness lurking within. I am, at most, on the third or fourth circle of hell.

## Terra Doesn't Actually Work On Windows

Saying that Terra supports Windows is a statement fraught with danger. It is a statement so full of holes that an entire screen door could try to sell you car insurance and it'd still be a safer bet than running Terra on Windows. Attempting to use Terra on Windows will work if you have Visual Studio 2015 installed. It _might_ work if you have Visual Studio 2013 installed. No other scenarios are supported, especially not ones that involve being productive. Actually _compiling_ Terra on Windows is a hellish endeavor comparable to climbing Mount Everest in a bathing suit, which requires either having Visual Studio 2015 installed _to the default location_, or manually modifying a Makefile with the exact absolute paths of all the relevant dependencies. At least up until last week, when I [submitted a pull request](https://github.com/zdevito/terra/pull/400) to minimize the amount of mountain climbing required.

The problem Terra runs into is that it tries to use a registry value to find the location of Visual Studio and then work out where `link.exe` is from there, then finds the include directories for the C runtime. This hasn't worked since Visual Studio 2017 and also requires custom handling for each version because compiling an iteration of Visual Studio apparently involves throwing the directory structure into the air, watching it land on the floor in a disorganized mess, and drawing lines between vaguely related concepts. Good for divining the true nature of the C library, bad for building directory structures. Unfortunately, should you somehow manage to compile Terra, it will abruptly stop working the moment you try to call `printf`, claiming that `printf` does not actually exist, even after importing `stdio.h`.

Many Terra tests assume that `printf` actually resolves to a concrete symbol. [This is not true](https://social.msdn.microsoft.com/Forums/vstudio/en-US/5150eeec-4427-440f-ab19-aecb26113d31/updated-to-vs-2015-and-now-get-unresolved-external-errors?forum=vcgeneral) and hasn't been true since Visual Studio 2015, which turned several `stdio.h` functions into inline-only implementations. In general, the C standard library is under no obligation to produce an actual concrete symbol for any function - or to make sense to a mere mortal, for that matter. In fact, it might be more productive to assume that the C standard was wrought from the unholy, broiling chaos of the void by Cthulhu himself, who saw fit to punish any being foolish enough to make reasonable assumptions about how C works.

Unfortunately, importing `stdio.h` does not fix this problem, for two reasons. One, Terra did not understand inline functions on windows. They were ephemeral wisps, [vanishing like a mote of dust on the wind](https://github.com/zdevito/terra/issues/401) the moment a C module was optimized. A [pull request fixed this](https://github.com/zdevito/terra/pull/405), but it can't fix the fact that the Windows SDK was wrought from the innocent blood of a thousand vivisected COMDAT objects. Microsoft's version of `stdio.h` can only be described as an extra-dimensional object, a meta-stable fragment of a past universe that can only be seen in brief fragments, never all at once.

Luckily for the Terra project, I am the demonic presence they need, for I was once a Microsoftie. Long ago, I walked the halls of the Operating Systems Group and helped craft black magic to sate the monster's unending hunger. I saw True Evil blossom in those dark rooms, like having only three flavors of sparkling water and a pasta station only open on Tuesdays.

I know the words of Black Speech that must be spoken to reveal the true nature of Windows. I know how to bend the rules of our prison, to craft a mighty workspace from the bowels within. After [fixing the cmake implementation](https://github.com/zdevito/terra/pull/322) to function correctly on Windows, I intend to perform [the unholy incantations](https://gist.github.com/tonetheman/522b623d00e64cb5feda5d68252fa68a) required to invoke the almighty powers of COM, so that it may find on which fifth-dimensional hyperplane Visual Studio exists. Only then can I disassociate myself from the mortal plane for long enough to [tackle the `stdio.h` problem](https://github.com/zdevito/terra/issues/401#issuecomment-537245442). You see, children, programming for Windows is easy! All you have to do is **s͏̷E͏l͏̢҉l̷ ̸̕͡Y͏o҉u͝R̨͘ ̶͝sơ̷͟Ul̴**

For those of you who actually wish to try Terra, but don't want to wait for ~~me to fix everything~~ a new release, you can embed the following code at the top of your root Terra script:

{{<pre lua>}}if os.getenv("VCINSTALLDIR") ~= nil then
  terralib.vshome = os.getenv("VCToolsInstallDir")
  if not terralib.vshome then
    terralib.vshome = os.getenv("VCINSTALLDIR")
    terralib.vclinker = terralib.vshome..[[BIN\x86_amd64\link.exe]]
  else
    terralib.vclinker = ([[%sbin\Host%s\%s\link.exe]]):format(terralib.vshome, os.getenv("VSCMD_ARG_HOST_ARCH"), os.getenv("VSCMD_ARG_TGT_ARCH"))
  end
  terralib.includepath = os.getenv("INCLUDE")

  function terralib.getvclinker()
    local vclib = os.getenv("LIB")
    local vcpath = terralib.vcpath or os.getenv("Path")
    vclib,vcpath = "LIB="..vclib,"Path="..vcpath
    return terralib.vclinker,vclib,vcpath
  end
end{{</pre>}}

Yes, we are literally overwriting parts of the compiler itself, at runtime, from our script. **Welcome to Lua!** Enjoy your stay, and don't let the fact that any script you run could completely rewrite the compiler keep you up at night!

## The Existential Horror of Terra Symbols

Symbols are one of the most slippery concepts introduced in Terra, despite their relative simplicity. When encountering a Terra Symbol, one usually finds it in a function that looks like this:

{{<pre lua>}}TkImpl.generate = function(skip, finish) return quote
    if [TkImpl.selfsym].count == 0 then goto [finish] end 
    [TkImpl.selfsym].count = [TkImpl.selfsym].count - 1
    [stype.generate(skip, finish)]
end end{{</pre>}}

Where `selfsym` is a symbol that was set elsewhere.

"Aha!" says our observant student, "a reference to a variable from an outside context!" This construct _does_ let you access a variable from another area of the same function, and using it to accomplish that will generally work as you expect, but what it's actually doing is much ~~worse~~ more subtle. You see, grasshopper, a symbol is not a reference to a variable node in the AST, it is a reference to an _identifier_.

{{<pre lua>}}local sym = symbol(int)
local inc = quote [sym] = [sym] + 1 end

terra foo()
  var [sym] = 0
  inc
  inc
  return [sym]
end

terra bar()
  var[sym] = 0
  inc
  inc
  inc
  return [sym]
end{{</pre>}}

Yes, that is valid Terra, and yes, the people who built this language did this on purpose. Why any human being still capable of love would ever design such a catastrophe is simply beyond me. Each symbol literally represents not a reference to a variable, but a `unique variable name` that will refer to any variable that has been initialized in the current Terra scope with that particular identifier. You aren't passing around variable _references_, you're passing around variable _names_.

These aren't just symbols, they're **typed preprocessor macros**. They are literally C preprocesor macros, capable of causing just as much woe and suffering as one, except that they are typed and they can't redefine existing terms. This is, admittedly, _slightly_ better than a normal C macro. However, seeing as there have been entire books written about humanity's collective hatred of C macros, this is equivalent to being a slightly more usable programming language than Brainfuck. This is such a low bar it's probably buried somewhere in the Mariana Trench.

## Terra is C but the Preprocessor is Lua

You realize now, the monstrosity we unleashed upon the world? The sin that Terra has committed now lies naked before us.

**Terra is C if you replaced the preprocessor with Lua.**

Remember how Terra says you can implement Java-like and Go-like class systems? You can't. Or rather, you will end up with a pathetic imitation, a facsimile of a real class system, striped down to the bone and bereft of any useful mechanisms. It is nothing more than an implementation of vtables, just like you would make in C. Because Terra is C. It's metaprogrammable C.

There can be no constructors, or destructors, or automatic initialization, or any sort of borrow checking analysis, because Terra has no scoping mechanisms. The only thing it provides is `defer`, which only operates inside lua lexical blocks (`do` and `end`)... sometimes, if you get lucky. The exact behavior is a bit confusing, and of course can only be divined by random experimentation because it **isn't documented anywhere!** Terra's only saving grace, the _singular keyword_ that allows you to attempt to build some sort of pretend object system, **isn't actually mentioned anywhere**.

Of course, Terra's metaprogramming _is_ turing complete, and it is _technically possible_ to implement some of these mechanisms, but only if you either wrap absolutely every single variable declaration in a function, or you introspect the AST and annotate every single variable with initialization statuses and then run a metaprogram over it to figure out when constructors or destructors or assignment operators need to be called. Except, this might not work, because the (undocumented, of course) `__update` metamethod that is supposed to trigger when you assign something to a variable has a bug where it's not always called in all situations. This turns catching assignments and extracting l-value or r-value status from a mind-bogglingly difficult, herculean task, to a near-impossible trial of cosmic proportions that probably requires the help of at least two Avengers.

## There Is No Type System

If Terra was actually trying to build a metaprogramming equivalent to templates, it would have an actual type system. These languages already exist - [Idris](https://www.idris-lang.org/), [Omega](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.121.4251), [F*](https://www.fstar-lang.org/), [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)), [Sage](https://sage.soe.ucsc.edu/), etc. but none of them are interested in using their dependent type systems to actually metaprogram low-level code (except [ATS](https://en.wikipedia.org/wiki/ATS_(programming_language)), which unfortunately still wants you to prove the correctness of your code). The problem is that building a recursively metaprogrammable type system requires building a proof assistant, and everyone is so proud of the fact they built a proof assistant they forget that dependent type systems can do other things too, like build really fast memcpy implementations.

Terra, on the other hand, provides only the briefest glimpse of a type system. Terra functions enjoy what is essentially a slightly more complex C type system. However, the higher-level Lua context is, well, Lua, which has exactly four types: Tables, Functions, Strings, and Numbers. That's it. Also, it's dynamic, not static, so everything is a syntax or a runtime error, because it's a scripting language. This means all your metaprogramming is sprinkled with type-verification calls like `:istype()` or `:isstruct()`, except the top came off the shaker and now the entire program is just sprinkles, everywhere. This is fine for when your metaprograms are, themselves, relatively simple. It is not fine when you are returning meta-programs out of meta-meta-functions.

This is the impasse I find myself at, and it is the answer to the question I know everyone wants to know the answer to. For the love of heaven and earth and all that lies between, **why am I still using Terra?**

The truth is that the project I'm working on requires highly complex metaprogramming techniques in order to properly generate type-safe mappings for arbitrary data structures. Explaining _why_ would be an entire blog post on it's own, but suffice to say, it's a complex user interface library that's intended to run on tiny embedded devices, which means I can't simply give up and use Idris, or indeed anything that involves garbage collection.

What I really want is a low-level, recursively metaprogrammable language that is also recursively type-safe, in that any type strata can safely manipulate the code of any layer beneath it, preferably via [algebriac subtyping](https://www.cl.cam.ac.uk/\~sd601/thesis.pdf) that ensures all types are recursively a subset of types that contain them, ad nauseam. This would then allow you to move from a "low-level" language to a "high-level" language by simply walking up the tower of abstraction, building meta-meta-programs that manipulate meta-programs that generate low-level programs.

Alas, such beauty can only exist in the minds of mathematicians and small kittens. While I may one day attempt to build such a language, it will be nothing more than a poor imitation, forever striving for an ideal it cannot reach, cursed with a vision from the gods of a pristine language no mortal can ever possess.

I wish to forge galaxies, to wield the power of computation and sail the cosmos upon an infinite wave of creativity. Instead, I spend untold hours toiling inside LLVM, wondering why it won't print "Hello World".

In conclusion, everything is terrible and I want to die.