+++
title = "The IM Failure"
date = 2010-11-18T03:56:00Z
updated = 2011-01-22T03:39:43Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

This is completely insane. First, Microsoft has rendered its new Windows Live Messenger (previously MSN messenger) almost completely useless.

 * No more handwriting
 * No more setting your name to anything other then your first and last name.
 * All links you click on redirect you to a page from Microsoft warning you about the dangers of the internet, requiring you to click a link to proceed.
 * All photosharing is now incompatible with previous versions of messenger, and instead of actually just sending the file, it will instead fail completely.
 * Any youtube video, image, or any other link you copy paste into the window will automatically trigger a sharing session whether you like it or not.
 * It will, at times, randomly decide none of your messages are getting through.
 * You can no longer have a one-sided webcam session. WLM will simply leave your webcam as a giant, useless blank image underneath your conversation partner, demanding that you buy a webcam.
 * It's new emoticons must have been influenced by H.R.Giger (you can't turn them off and leave custom ones on).

Naturally I wasn't able to put up with this for very long and moved to pidgin. Pidgin has many issues of its own, including an ass-backwards UI design and this really annoying tendency to create a new popup window to confirm every single file transfer, among other truly bizarre UI design elements. I was willing to put up with these, because honestly pidgin is designed for Linux and just happens to work on windows too, and their libpurple library is the basis of almost all open-source IM clients.

Pidgin, however, has now stopped working as well. It now spastically refuses to connect to the MSN service because the SSL certificate is invalid. I can appreciate it trying to protect my privacy, but there is no way to override this. So, pidgin is now out of the question.

Well, how about Trillian? During its installation, Trillian informed me that "The Adobe Flash 9.0 plugin for Internet Explorer could not be found. This plugin is required for some of Trillian's features."

Trillian is no longer on my computer.

After digging around on wikipedia, I came up with Miranda IM, which seemed to be my last hope for a multi-protocol service that didn't suck total ass. It supported WLM, AIM, and... not google talk? Not XMPP, the most useful extensible open source protocol? Um, ok. Its UI design is more compact then Pidgin but arguably even worse, and it doesn't support custom emoticons or hardly ANYTHING on the WLM protocol. It served its purpose by at least letting me log the fuck in like I wanted to, though.

This is driving me up the wall. If anything else happens, I'm going to snap and simply *make time* to work on ***my own*** IM client implementation that doesn't have **glaring design flaws** like *every single other one*. Honestly, requiring the Internet Explorer flash plugin to run everything? What the fuck are they smoking? What the hell is wrong with these developers?!

If only D had a good development environment, then I could write my IM client in it.
