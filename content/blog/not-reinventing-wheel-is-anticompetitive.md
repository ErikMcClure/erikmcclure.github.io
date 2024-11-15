+++
blogimport = true
categories = ["blog"]
comments = [2580492638773785216, 1465072138398960621, 1883344438890843204, 4981196591329632074, 4567640033476427600, 1918972859475533854]
date = "2014-11-22T00:15:00Z"
title = "Never Reinventing The Wheel Is Anticompetitive "
updated = "2014-11-22T12:39:24.000+00:00"
draft = true
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{%blockquote%}}*Reality is that which, when you stop believing in it, doesn't go away.*
 ― Philip K. Dick{{%/blockquote%}}

Nowadays, you aren't supposed to write anything yourself. In the world of open-source software, you're supposed to find a library that does what you want, and if that doesn't work, commit a change that makes it work. Writing your own library is a bad thing because it's perceived as redundant if there's already a library that does most of what you want. The problem is that this is inherently anti-competitive. If there's only one library that does something, you have a monopoly, and then you get [Heartbleed](http://en.wikipedia.org/wiki/Heartbleed), because there's zero diversification. However, if I try to invent my own wheel, I'll just be scolded and told to contribute to an existing wheel instead, even if my wheel needs to have giant spikes on it.

I'm beginning to wonder if these people actually understand how software works. Code is not some kind of magical, amorphous blob that can just *get better*. It's a skyscraper, built on top of layers and layers of code, all sitting on top of a foundation that dictates the direction the code evolves in. If someone invents a better way to architect your program, you can't just swap pieces of it out anymore, because you are literally *changing how the pieces fit together* in order to improve the program. Now, I'm sure many coders would say that, if there's a better architecture, you should just rip the entire program apart and rebuild it the better way.

The problem is that *there is no one-size fits all solution* for a given task. **Ever**. This is true for any discipline, and it is still true in the magical world of programming no matter how loudly you shout "LALALALALALA" while plugging your ears. By *definition* you cannot create a software library that is constructed in two different ways, because that is literally *two different software libraries*. Thus, just like how we have many different wheels for different situations, it is perfectly fine to reinvent the wheel to better suit the problem you are tackling instead of trying to extend an existing library to do what you want, because otherwise you'll end up with a bloated piece of crap that still doesn't do what you want. If you actually believe that it is possible to write a vector math library that covers all possible use scenarios for every possible solution, then I invite you to put monster truck tires on your smartcar and tell me how well that works. We might as well ask the Flying Spaghetti Monster to solve our problems instead.

You *want* there to be several different libraries for rendering vector graphics. You *want* there to be many different programming languages. You *want* there to be multiple text layout engines. These things create competition, and as each codebase vies to be used by more and more people, each will start catering to a slightly different set of problems. This creates lots of different tools that programmers can use, so instead of having to use their Super Awesome Mega Swiss Army Knife 4000, they can just use a butter knife because *that's all they goddamn need*.

At this point, most coders will struggle to defend their position by falling back on the tried and true saying of "well, for most programmers, this is perfectly reasonable." Exactly what do they mean by *most programmers*? Because *most programmers* use [C++, C, and Java](http://spectrum.ieee.org/computing/software/top-10-programming-languages), and are probably maintaining systems that are 20 years old and couldn't possibly apply anything they were just told in any realistic scenario. Do they just mean programmers who are making hip WebGL applications? What arbitrary subset of programmers are we talking about here? Of course, at this point, it doesn't matter, because they are literally cherry picking whatever programming community supports their opinion, and we're praying to the Flying Spaghetti Monster again.

Diversity is important. I am tired of programmers living in some fantasy world where all diversity has been eliminated and everyone follows the One True Path, and uses the One True Library for each individual task. This is not a world I want to live in, because *my problems are not your problems*, and even if your library is amazing at solving your problems, it probably won't solve mine. I'll go even further and say that this kind of thinking is *dangerous*. It reeks of cargo-cult coding and institutionalized knowledge designed to destroy creativity.

When people say you shouldn't re-invent the wheel, they mean you shouldn't re-invent the wheel if that's *not what you're trying to do*. If your main goal is to make a game, you shouldn't write a graphics engine. If your main goal is to make a webpage, you shouldn't write a webpage designer. However, if you just finished a game and think "Gee, that graphics engine was terrible, I'm going to make a better one," then writing your own graphics engine is *perfectly fine*.

Including the entire jQuery library just so you can have the {{<code>}}$("id"){{</code>}} shortcut is like driving a smartcar with spiked monster truck wheels down a freeway. It's dangerous, stupid, and completely unnecessary.
