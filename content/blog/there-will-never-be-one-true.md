+++
blogimport = true
categories = ["blog"]
date = "2015-09-28T03:07:00Z"
title = "There Will Never Be One True Programming Language"
updated = "2015-09-28T05:13:08.000+00:00"
draft = true
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
A disturbing number of otherwise intelligent programmers seem to believe that, one day in the distant future, everyone will use the same magical programming language that solves everyone's problems at the same time. Often, this involves garbage collection, with lots of hand-waving about computers built with [Applied Phlebotinum](http://tvtropes.org/pmwiki/pmwiki.php/Main/AppliedPhlebotinum).

For the sake of argument, let's assume it is the year 30XX, and we are a budding software developer on Mars. We have quantum computers that run on magic pixie dust and let us calculate almost anything we want as fast as we want so long as we don't break any laws of physics. Does everyone use the same programming language?

***No.*** Obviously, even if we built quantum computers capable of simulating a classical computer a thousand times faster than normal for some arbitrary reason{{<sup>}}<a href="#foot1">[1]</a>{{</sup>}}, we would still want to write software in a way that was easy to comprehend, and qubits are [anything but](https://en.wikipedia.org/wiki/Qubit). So, the standard programming language our Mars programmer would be using would not interact with the quantum computer at all. Perhaps it would be some form of functional language, which the cool kids seem to think will save the entire software industry and make everything faster, better, more reliable and probably cure cancer in the process{{<sup>}}<a href="#foot2">[2]</a>{{</sup>}}.

But now, this same programming language that allows you to ignore the quantum computer must also be used to write it's own compiler that runs on... a quantum computer. So it needs to simultaneously be an elegant language for writing quantum programs. It also needs to be the perfect language for writing games and business applications and hypernet sites and machinery and brain implants and rocket trajectories and robot AI and planetary weather simulators and life support systems and hologram emittors and&mdash;

... Wouldn't it be a lot easier to just have domain-specific languages?

If you want to get technical, you can do all that with Lisp. It's just lambda calculus. It's also a *giant pain in the ass* when you use it for things that it was never meant for, like low level drivers or game development. You could try extending Lisp to better handle those edge-cases, and that's exactly what many lisp-derivatives do. The problem is that **we will always be able to find new edge-cases**. So, you would have to continue bolting things on to your magical language of ultimate power, playing a game of context-free whack-a-grammar for the rest of eternity.

This is ultimately doomed to fail, because it ignores the underlying purpose of programming languages: A programming language is a **method of communication**. Given two grammars, one that can do everything versus one that can do the one thing you're trying to do *really well*, which one are you going to pick for that particular project? Trying to get everyone to use the same language while they are trying to solve radically different problems is simply *bad engineering*. Do you *really* want to write your SQL queries using Lisp? You can only stuff so many tools into a swiss army knife before it becomes [completely impractical](http://www.wengerna.com/stuff/contentmgr/files/0/a45137daa224e9531cb3050458faee64/image/wenger_giant_knife.png). 

Of course, a language that could do *everything* would also allow you to define any arbitrary new language within it, but this is equivalent to building an entire new language from scratch, because now everyone who wants to contribute to your project has to learn your unique grammar. However, having a common ground for different languages is a powerful tool, and we are headed in that direction with LLVM. You wouldn't want to write a program in LLVM, but it sure makes writing a compiler easier. 

Instead of a future with one true programming language, perhaps we should be focused on a future with **standard low-level assembly**. A future with many different languages that can all talk to each other. A future where we don't *need* to argue about which programming language is the best, because every language could be used for whatever task it is best suited for, all in the same project. Different compilers could interpret the standard low-level assembly in their own ways, optimizing for different use-cases.

A universal standard for low-level assembly would **solve everyth&mdash;**

{{<img src="https://imgs.xkcd.com/comics/standards.png" alt="mandatory xkcd" width="500" title="Mars insisted on making semicolons optional, which rendered their code completely incompatible with Earth's code. The resulting Great Semicolon War left the entire northern hemisphere of Earth uninhabitable." >}}

... Actually, nevermind{{<sup>}}<a href="#foot3">[3]</a>{{</sup>}}.

<hr>
{{<span style="font-size:80%">}}<sup id="foot1">1</sup></a> As of right now, this is not actually true for the vast majority of computations we know how to do with qubits. We know that <i>certain classes of problems</i> are exponentially faster with quantum computers, but many other functions would get a mere linear increase in speed, <i>at most</i>. Whether or not this will change in the future is an active area of research.
<sup id="foot2">2</sup> They <i>totally</i> didn't say the same thing about object-oriented programming 30 years ago.
<sup id="foot3">3</sup> <i></sarcasm></i>
{{</span>}}
