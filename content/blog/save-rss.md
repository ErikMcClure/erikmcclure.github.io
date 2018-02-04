+++
title = "Save RSS"
date = 2011-05-08T22:03:00Z
updated = 2011-05-08T22:03:57Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

A recent article by Jesse Stay describes how [Twitter and Facebook are killing RSS](http://www.staynalive.com/2011/05/twitter-and-facebook-both-quietly-kill.html?q=1). Many people point out that you don't really *need* an RSS feed for either service, since the content they generate is not particularly conducive to an RSS feed. While this is strictly true if you are attempting to read the actual RSS feed, I don't consider this the primary benefit of an RSS feed. 

RSS feeds provide a way to broadcast data in a standard format, which then allows an application to be quickly constructed using an RSS parser to process that data. Since its an open standard, efficient, open-source RSS parsers propagate rapidly. This allows creative use of RSS feeds to comb vastly different types of media, and more importantly *allows inter-site integration* through a single API. That is, if you publish your blog on an RSS feed, scrapers on Last.fm can pick it up and display it on your artist page. The reason RSS is so valuable is because it provides a standard method of communicating contextualized data over the web.

An interesting consequence is that few people actually benefit from the human-readability of RSS. Hence, if there is ever an RSS 3.0, it would benefit greatly from a compressed machine version (perhaps utilizing something like Google's [protocol buffers](http://code.google.com/p/protobuf/)) for fast and efficient data transfer. Proper caching is also important, so an RSS feed is only served up when its been updated. Being able to directly indicate data formats like video and mp3 would also help to enforce more global methods of parsing them (an example being the very useful RSS feed that Newgrounds provides on [my audio page](http://rss.ngfiles.com/users/1743000/blackhole12/audio/)). This is a more general problem with XML based formats, however, and is best left for a separate debate.

Either way, while RSS as a standard publication method may be dying, the internet must strive to keep it available for API developers, because **RSS promotes freedom of information** by providing an easy way to process that information. The interconnectivity of the internet and the freedom of information that it experiences cannot be provided by a proprietary protocol. RSS hasn't even gotten near its full potential and its already dying. Do your part to save RSS by making your website's information accessible via RSS, even if its only in the metadata. The user interface to RSS may be facing extinction, but the information parsing it provides must not be allowed to die.
