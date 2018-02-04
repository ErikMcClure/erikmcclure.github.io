+++
blogimport = true
categories = ["blog"]
date = "2013-02-24T20:54:00Z"
title = "The Dark Side of Web Development"
updated = "2013-02-25T11:19:29.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I am, by no means, a very good web developer. I am, however, a highly experienced C++ developer. The end result is that people usually ask me why I like C++ so much when everyone else hates it, and everyone is supposedly in agreement that C++ is the worst language ever made and unless you are writing drivers you should never, ever use it. I mean, you can write games in Python now, right?

While these kinds of ignorant opinions are frustrating for me and my fellow C++ developers who actually need efficient cross-platform programs, that's not what I'm here to debate. In fact, if you think C++ is a complete piece of shit, I won't argue with you about that at all. For all I know, you may be right!

Unfortunately for you, **web development is just as bad as C++**.

Of course, I just said that I'm not very good at web development, so doesn't this disqualify me from having an opinion about it? On the contrary, the fact that I'm not very good at web development is extremely important. The reason is that one of the major complaints about C++ is that *good C++ code is hard to write if you are an inexperienced developer*. I can certainly attest to that fact, it's taken me years to develop my C++ skills to this point. The fact that Python code is so easy and natural to write is one of its major selling points.

The thing is, *good HTML/CSS code is also hard to write if you are an inexperienced developer*. I can attest to this too, because I'm an inexperienced developer, and its *really goddamn hard* to write good HTML/CSS code. Let me clarify what I mean by "good" here: by "good" I mean code that works on all platforms, doesn't blow up, and doesn't contain strange edge cases that will cause it to fail for no reason. Many developers preach the joys of functional programming, which eliminates most edge cases by prohibiting functions that have side-effects.

Everyone knows about the subtle, nasty bugs that can crop up in C++, often as a result of doing something that was intuitive, yet wrong. This is rehashed in the "C++ is bad" arguments over and over and over without end, *especially* with the functional programming zealots. The problem is that HTML/CSS has *all sorts of really stupid crap* that can happen when you do something subtly wrong with the HTML that is not immediately obvious to a new developer.

It took me a while to learn that you shouldn't be putting block level elements inside inline elements in your HTML - it will *almost always* work, until it doesn't, and it won't be obvious what exactly is going wrong. Many people new to HTML aren't even aware of HTML resets, so they'll have to try and remember which elements are block-level by default and which aren't. Then the new standard introduced a bunch of psuedo-block level elements that try to fix some of these problems, except they aren't supported by everything because *MICROSOFT*, so now you're stuck with a bunch of workarounds for IE that take forever to develop.

Just recently I discovered an amazingly weird bug with my webpage, wherein upon first loading it, all my buttons would be stacked vertically. It appeared that Chrome was ignoring {{<code>}}float:left{{</code>}} on those elements... until the page was refreshed or any other part of the website was visited, at which point everything started working again! Firefox, of course, didn't have any issues with it. The problem was that I had a {{<code>}}<p>{{</code>}} element defined as a giant button, then put {{<code>}}<a href=""></a>{{</code>}} around it because I wanted the entire button to be a link. This, of course, violates the inline/block element mandate I outlined above, despite it being a very natural thing to do - you want the entire button to link somewhere? Put a link around it. It'll *almost always* work, until it doesn't.

Trying to write perfectly standards conformant HTML is exceedingly difficult because half the time what you want to do isn't in the standard or is some hack that works almost all the time but not quite. The rest of the time, the "standard" way of doing things is completely and utterly bizarre, like using {{<code>}}margin:auto 0;{{</code>}} to center elements. But wait, that only works on horizontal elements! If you want to vertically center an arbitrary element, you have to use [a lot of weird crap](http://www.jakpsatweb.cz/css/css-vertical-center-solution.html) just to get it to work on everything. Or you could just use a table. But tables are bad, so you should never use them, even though they actually solve a whole lot of problems new developers have because their non-table solutions are completely unintuitive, because we're all trying to use a *document layout engine* to make *GUIs*, for crying out loud.

Many web developers argue that these problems are going away now that we have a better standard. Sadly, they are just getting started - WebGL will become exceedingly important in the next 5 years, and its standardization is an *[absolute mess](http://codeflow.org/entries/2013/feb/22/how-to-write-portable-webgl/#why-portable)* almost to the point of it being unusable. Then there's the sordid situation with HTML5 audio and video, which is only just starting to get tolerable. These problems are not going away - they are simply being replaced by different problems. Of course, some stuff actually is becoming easier in HTML - just like a lot of stuff is becoming easier in C++11. So either you ignore both the new C++ features and the new HTML features, or you admit that C++ has become less horrible recently.

Of course, C++ is still way too easy to write unmaintainable code in, and HTML/CSS code is clearly self-documenting! Except it obviously isn't, given the endless horror stories of terrifying CSS dependencies with random {{<code>}}!important{{</code>}} keywords flung around in a giant mess that's just as impossible to understand as templatized C++ hell.

But wait, if you just write proper HTML, it won't have that problem! Well, *duh*, if you write proper C++, you don't have that problem either. I'm sorry, if you are going to hate something, you can't have your cake and eat it too. If C++ supporting features that can be abused to make code impossible to read, like operator overloading, makes it a bad language, then CSS is a bad language, because there is an endless list of obscure, bizarre syntax you can use in your CSS style sheets (like {{<code>}}!important{{</code>}}) that will make your HTML almost impossible to read. But wait, C++ is also incredibly hard to parse! HTML being easy to parse must be why Opera recently gave up trying to maintain its own HTML layout engine... Oh wait. Well at least javascript is a consistent, easy to understand language that doesn't have any weird quirks... [JUST KIDDING!](http://www.youtube.com/watch?v=_yZHbh396rc)

So it seems modern technology has succeeded in replacing a very fast, difficult to use, inconsistent language with 3 different, very slow, difficult to use, inconsistent languages.

Now, if you want to believe that HTML/CSS is still good when used correctly, then all I ask is that you grudgingly admit that, maybe, *just maybe*, C++ is also good when used correctly. If you still think C++ is an abomination from the depths of cthulu, I hope you can admit that web development therefore must also be an abomination.

Or we can just keep arguing with each other, because that's always productive.
