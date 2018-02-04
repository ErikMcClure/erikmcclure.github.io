+++
blogimport = true
categories = ["blog"]
date = "2012-02-05T20:59:00Z"
title = "'Programmer' is an Overgeneralization"
updated = "2012-02-06T05:21:33.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{%blockquote%}}*"Beware of bugs in the above code; I have only proved it correct, not tried it."* - Donald Knuth{{%/blockquote%}}

Earlier today, I came across a post during a *google-fu* session that claimed that no one should use the C++ standard library function {{<code>}}make_heap{{</code>}}, because almost nobody uses it correctly. I immediately started mentally ranting about how utterly ridiculous this claim is, because anyone whose gone to a basic algorithm class would know how to properly use {{<code>}}make_heap{{</code>}}. Then I started thinking about all the programmers who *don't* know what a heap is, and furthermore probably don't even need to know. 

Then I realized that both of these groups are still called programmers. 

When I was a wee little lad, I was given a lot of very bad advice on proper programming techniques. Over the years, I have observed that most of the advice wasn't actually bad advice in-of-itself, but rather it was being given without context. The current startup wave has had an interesting effect of causing a lot of hackers to realize that "performance doesn't matter" is a piece of advice *riddled* with caveats and subtle context, especially when dealing with complex architectures that can interact in unexpected ways. While this [broken telephone effect](http://en.wikipedia.org/wiki/Chinese_whispers) arising from the lack of context is a widespread problem on its own, in reality it is simply a symptom of an even deeper problem. 

The word programmer covers a stupendously large spectrum of abilities and skill levels. On a vertical axis, a programmer could barely know how to use vbscript, or they could be writing compilers for Intel and developing scientific computation software for aviation companies. On a horizontal axis, they could be experts on databases, or weeding performance out of a GPU, or building concurrent processing libraries, or making physics engines, or doing image processing, or generating 3D models, or writing printer drivers, or using coffeescript, HTML5 and AJAX to build web apps, or using nginx and PHP for writing the LAMP stack the web app is sitting on, or maybe they write networking libraries or do artificial intelligence research. They are all programmers. 

**This is insane.**

Our world is [being eaten by software](http://online.wsj.com/article/SB10001424053111903480904576512250915629460.html). In the future, programming will be a basic course alongside reading and math. You'll have four R's - Reading, 'Riting, 'Rithematic, and Recursion. Saying that one is a programmer will become meaningless because 10% or more of the population will be a programmer on some level. Already the word "programmer" has so many possible meanings it's like calling yourself a "scientist" instead of a physicist. Yet, what other choices do we have? The only current attempt at fixing this gave a paltry [3 options](http://www.skorks.com/2010/03/the-difference-between-a-developer-a-programmer-and-a-computer-scientist/) that are just incapable of conveying the differences between me and someone who graduated from college with a PhD in artificial intelligence. They do multidimensional mathematical analysis and evaluation using functional languages I will never understand without years of research. I'm supposed to write incredibly fast, clever C++ and HLSL assembly while juggling complex transformation matrices to draw pretty pictures on the screen. These jobs are both extremely difficult for completely different reasons, and neither person can do the other persons job. What is good practice for one is an abhorration for the other. We are both programmers. Even *within our own field*, we are simply graphics programmers or AI programmers or {{<code>}}[x]{{</code>}} programmers. 

Do you know why we have pointless language wars, and meaningless arguments about what is good in practice? Do you know why nobody ever comes to a consensus on these views except in certain circles where "practice" means the same thing to everyone? Because we are overgeneralizing *ourselves*. We view ourselves as a bunch of programmers who happen to specialize in certain things, and we are making the mistake of thinking that our viewpoint applies outside of our area of expertise. We are industrial engineers trying to tell chemists how to run their experiments. We are architects trying to tell English majors how to design an essay because we both use lots of paper. 

This attitude is deeply ingrained in the core of computer science. The entire point of computer science is that a bunch of basic data structures can do everything you will ever need to do. It is a fallacy to try and extend this to programming in general, because it simply is not true. We are forgetting that these data structures only do everything we need to do in the magical perfect land of mathematics, and ignore all the different implementations that are built for different areas of programming, for completely different uses. Donald Knuth understood the difference between theory and implementation - we should strive to recognize the difference between *theoretical* and *implementation-specific* advice. 

It is no longer enough to simply ask someone if they are a programmer. Saying a programmer writes programs is like saying a scientist does science. The difference is that botanists don't design nuclear reactors.
