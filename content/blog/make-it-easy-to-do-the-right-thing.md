+++
blogimport = true
categories = ["blog"]
date = "2015-07-19T23:58:00Z"
draft = true
title = "Make It Easy To Do The Right Thing"
updated = "2015-07-19T23:58:51.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{%blockquote%}}*"But MongoDB is web scale!"*{{%/blockquote%}}

It seems that the general consensus nowadays is that using MongoDB is a bad idea. I don't really have much of an opinion on this particular argument, mostly because I never understood NoSQL in the first place. "With MongoDB, you don't have to worry about schemas or relationships between data!"

...but, all my data IS relational. In fact, most data is. That's *why* we built relational databases in the first place. Data only makes sense when related to other data and presented in some coherent context. The fact that some people thought websites didn't involve relational data when basically every website in existence has to maintain a table of users mapped to the content they produce is mind-boggling. It seems that we have finally reached a point where people have [re-discovered this](http://cryto.net/~joepie91/blog/2015/07/19/why-you-should-never-ever-ever-use-mongodb/), and that using a relational database will make your life much easier in the long run. Really.

However, there's still one hold out, one reason to use MongoDB despite the fact that we know using a non-relational database is a bad idea.

{{%blockquote%}}"But building relational databases is hard!"{{%/blockquote%}}

If that's true, ***then make it easier!*** I am continually dumbfounded by the tendency of software engineers to find ways to avoid doing the right thing if it's too difficult, instead of finding a way to *make the right thing easier*. If making a relational database will save time in the long run, but no one wants to set it up because it's difficult and time-consuming, *find a way to make it less time-consuming*. 

One of the most egregious offenders of this is PHP. PHP made it easy to write websites. It did this by making it easy to do everything the wrong way. As many people have discovered over the years, PHP makes it [astonishingly difficult](http://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design/) to do the right thing. As users of MongoDB discovered, all this accomplished was making setting up the initial framework easy, at the cost of future scalability. Other frameworks popped up that made it easier to do things the *right* way (I am particularly a fan of Go, despite it's shortcomings). 
