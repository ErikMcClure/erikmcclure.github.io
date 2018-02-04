+++
title = "Go Build Is Not Predictable"
date = 2017-09-28T03:11:00Z
updated = 2017-09-28T03:11:00Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Go is cross-platform in the sense that I can compile a Go program on both Windows and Linux without having to change any code. Of course, since the operating system is different, we can expect some behavioral changes, but if I compile a Go program on two different Linux boxes, both with the exact same operating system and exact same Go installation path and exact same Go environment variables, one would expect the Go compiler to behave *deterministically*, such that the resulting programs would be identical.

This is not the case.

Now, at first this claim seems patently absurd, since computers themselves are deterministic. Unfortunately, as most of us have been made painfully aware of, something only seems "deterministic", and thus predictable, if we actually know all its input sources. An operating system like Windows is so ludicrously complex it might as well be a magic 8-ball summoned from the depths of the abyss, wrenched from the claws of Cthulu himself and thrown into a hurricane for some extra bits of entropy. Given all the things an operating system has to do, coupled with the fact that the hardware itself may sometimes be the source of the discrepancies, such behavior might be forgiven (but Cthulu probably wants his magic 8-ball back). However, in the context of a *compiler* for a modern language, I not only *expect* it to behave in a predictable fashion, I *depend* on it to do so. This is not particularly difficult to do for most compilers, because most compilers just take a bunch of command line parameters, and the only discrepancies you'll hit are when the default values for some of them change. Most of the time, if you set all your command line parameters, you will get predictable behavior.

Under certain circumstances, Go's compiler manages to break one of the core tenets of the language itself: the concurrency model. It all starts with the DNS resolver. If you are using Go for making any kind of web-oriented program (which is what Go is good at), and you do anything that involves domain names (which is basically everything), you're using the DNS resolver. The problem is that Go's compiler uses a *completely different DNS resolver* depending on the existence of random files it found scattered around your operating system. Here is the full list of conditions for when the CGo DNS resolver is used:

{{%blockquote%}}When cgo is available, the cgo-based resolver is used instead under a variety of conditions: on systems that do not let programs make direct DNS requests (OS X), when the LOCALDOMAIN environment variable is present (even if empty), when the RES_OPTIONS or HOSTALIASES environment variable is non-empty, when the ASR_CONFIG environment variable is non-empty (OpenBSD only), when /etc/resolv.conf or /etc/nsswitch.conf specify the use of features that the Go resolver does not implement, and when the name being looked up ends in .local or is an mDNS name. {{%/blockquote%}}

Naturally, if you don't use CGo anywhere in your program, you would expect not to be linked to it. You would also be wrong, because if any of the above conditions are true, Go decides to use CGo *anyway* and links to a completely different DNS resolver package. Normally this wouldn't really matter other than being weird, disturbing, and deeply unnerving, except for an itsy-bitsy tiny detail with how CGo works. **The C API forces CGo to create an entire new OS-level thread for each DNS request.**

Go is built around green threads, which are micro-threads that let you spin up a thousand or so of them without any noticeable problem. Guess what happens when all those threads make a DNS request? 

{{%blockquote%}}{{<pre>}}pthread_create failed: Resource temporarily unavailable{{</pre>}}{{%/blockquote%}}

Suddenly you have 1000 *actual threads* (or at least, you have an attempt
