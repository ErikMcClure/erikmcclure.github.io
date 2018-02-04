+++
title = "Is There A Commercial Open Source License?"
date = 2015-03-16T03:25:00Z
updated = 2015-03-16T03:44:58Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

{{%blockquote%}}*"Any headline that ends in a question mark can be answered by the word 'No'."* - Davis' law{{%/blockquote%}}

Putting *Commercial* and *Open-Source* together is often considered an oxymoron. Part of this is caused by constant confusion between the terms Open-Source and Free Software, which is made even worse by people who have more liberal interpretations of the phrase "Open-Source". In many cases, keeping the source code of a product proprietary serves no other purpose than to prevent people from stealing the product. Putting an application under the GPL and selling it is perfectly reasonable for software aimed at end-users, whom are unlikely to know how to compile the freely available source. Libraries aimed at developers, however, usually must resort to [Dual Licensing](http://en.wikipedia.org/wiki/Multi-licensing).

To me, dual licensing is a bit of a hack job. Using two fundamentally incompatible licenses at the same time always rubbed me the wrong way, even if it's perfectly legal for the creator of the software. The other problem is that this is usually done via copyleft licenses, and I would prefer more permissive licenses, given that I only care about whether or not the result is commercial or not. This, however, turns out to be a bit of a problem.

My ideal license for a commercial library would state that anyone is free to modify, distribute, or use the source code in anything they want, provided the resulting source code retains the provided license. If the resulting product is strictly noncommercial, it would be free. If, however, the source code or any derivation of the source code is used in a commercial product, there would be a fee. It'd basically be an MIT license with an added fee for commercial derivative works, with additional restrictions on what commercial derivative works are allowed.

The problem is, even if I were to use a dual-licensing strategy, there is no open-source license that contains a noncommercial restriction, because [this isn't open-source](http://opensource.org/docs/osd). Specifically, it violates Freedom 1 as defined by OSI:

{{%blockquote%}}1. The license **shall not restrict any party from selling** or giving away the software as a component of an aggregate software distribution containing programs from several different sources. **The license shall not require a royalty or other fee for such sale.**{{%/blockquote%}}

So, the *first sentence* of the definition of open-source software has made it abundantly clear that my software isn't open-source because I'm trying to make money off of it. This seems overly restrictive to me, because the source code is still freely available for people to distribute and modify, just not sell. Even then, they can still sell things made with the source code, they just pay a royalty or fee for it.

Now, there are good reasons why actual *free software* would forbid this. For example, if someone made an open-source library that required a royalty for commercial use whose code was repeatedly modified over and over again until it crept into all sorts of projects, it could then claim royalties on all of those products, even if only a few lines of its source code were used. This is obviously not conducive to creating a culture of sharing code.

*However*, there is another benefit of open-source software, which is now becoming progressively more obvious. It's the fact that, if you have the source code and compile it yourself, you can verify the NSA (probably) hasn't injected a backdoor into it. This is a benefit of *visibility*, and I think it should be encouraged. The problem is that the restrictions placed on free software are too harsh for most commercial libraries, who will then often resort to simply being proprietary.

So, sadly, there are no open-source commercial licenses, because those aren't *open-source*. Perhaps we need a new term, for software whose source is *accessible* but has usage restrictions. Even then, it's not entirely apparent what such a license would look like, or if sites like GitHub would actually allow it on public repositories. I took a peek at the [Unreal Engine 4 license](https://www.unrealengine.com/eula), but despite claiming to have it's source code available on github, it's actually in a [private repository](https://github.com/EpicGames/UnrealEngine) you must gain access to. To make matters worse, the actual Unreal Engine 4 license is *incredibly* restrictive. You are only allowed to distribute the engine code to other people who have agreed to the license! This obviously isn't what I want, but apparently no one else seems to think that software that's *kind of* open-source is actually valuable.

It's an awful shame, because I really don't want to make my project proprietary, but right now I don't have much choice. As far as I'm concerned, releasing something under a restricted open-source license is preferable to making it entirely proprietary. Unfortunately, the loudest programmers are also the least likely to be willing to compromise over ideological divides.
