+++
blogimport = true
categories = ["blog"]
comments = [4671761147535471189]
date = "2015-02-11T01:32:00Z"
title = "Why Don't You Just Fire Them?"
updated = "2015-02-11T01:32:31.000+00:00"
draft = true
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{%blockquote%}}*"Nothing is foolproof to a sufficiently talented fool."*
&nbsp; &mdash; Anonymous{{%/blockquote%}}

Programmers *love* to bash things like templates and multiple-inheritance and operator overloading, saying that they are abused too often and must be unilaterally banned, going so far as to design them out of their programming languages thinking this is somehow going to save them.

This makes about as much sense as saying that bank robbers use cars to escape police too much, so we need to ban cars.

Templates, fundamentally, are very useful and perfectly reasonable things to use in most sane contexts. They are used for things like vectors, where you want an int vector and a float vector. However, people point to the horrible monstrosities like Boost and say "Look at all the havoc templates have wrought!" Well, yeah. That's what happens when you abuse something. You can abuse anything, trust me. You simply cannot write a programming language that's idiot proof, because if you make *anything* idiot proof, someone will just make a better idiot.

Multiple inheritance is usually useful for exactly one thing: taking two distinct object inheritance lines and combining them. If you ever inherit more than two things at once, you probably did something wrong. The problems arise when you start inheriting 8 things and create gigantic inheritance trees with diamonds all over the place. Of course, you can build something just as confusing and unmaintable with single-inheritance (just look at the .net framework), but the point is that the language doesn't have a problem for letting you do this, *you* have an architectural issue because you're being an idiot.

You *do* have code reviews, right? You do realize you can just tell programmers to not do this, or simply not use a library clearly written by [complete maniacs](http://www.boost.org/)? Chances are you probably shouldn't have more than one or two template arguments or inherited classes, and you really shouldn't overload the + operator to subtract things. If you do, someone should tell you you're being an idiot in a code review, and if you keep doing it, they should just ***fire you***.

What really bothers me about these constant attacks on various language features or methodologies is that nearly all of them are [Slippery Slope fallacies](http://en.wikipedia.org/wiki/Slippery_slope). If we let programmers do this, they'll just make something more and more complicated until nobody can use it anymore! It's the same exact argument used for *banning gay marriage!* If your response to a programmer abusing a feature is to remove the feature, I really have to ask, **why don't you just fire them?** The programmer is the problem here. If anyone succeeds in checking awful code into your code base, you either have a systemic failure in your process, or you've hired an idiot that needs to be fired.

Programming languages are toolboxes. I want my array of tools to be powerful and adaptable, not artificially castrated because other programmers can't resist the temptation to build leaning towers of inheritance. It's like forcing all your carpenters to use hammers without claws, or banning swiss army knives because someone was using it wrong. If someone is using a tool wrong, it's because they haven't been trained properly, or they're incompetent. The mindset of banning problematic features in programming languages arises from coders who have to maintain bad code, and who are deluding themselves into thinking that if they got rid of those pesky templates, their lives would be much easier.

Having personally seen a visual basic program that succeeded in being almost impossible to decipher even after several weeks, I can assure you this is not the case, and never will be. The problem is that it's bad code, not that it's written in C++.
