+++
blogimport = true
categories = ["blog"]
date = "2017-11-01T15:26:00Z"
title = "Ignoring Outliers Creates Racist Algorithms"
updated = "2017-11-01T15:33:38.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Have you built an algorithm that *mostly* works? Does it account for *almost* everyone's needs, save for a few weird outliers that you ignore because they make up 0.0001% of the population? Congratulations, your algorithm is racist! To illustrate how this happens, let's take a recent example from Facebook. My friend's message was removed for "violating community standards". Now, my friend has had all sorts of ridiculous problems with Facebook, so to test my theory, I posted the exact same message on my page, and then had him report it.

{{< img src="/img/fb1.png" alt="" width="637" >}}
{{< img src="/img/fb2.png" alt="" width="512" >}}
{{< img src="/img/fb3.png" alt="" width="689" >}}

Golly gee, look at that, Facebook confirmed the message I sent does **not** violate community guidelines, but he's still banned for 12 hours for posting *the exact same thing*. What I suspect happened is this: Facebook has gotten mad at my friend for having a weird name multiple times, but he can't prove what his name is because he doesn't have access to his birth certificate because of family problems, *and* he thinks someone's been falsely reporting a bunch of his messages. The algorithm for determining whether or not something is "bad" probably took these misleading inputs, combined it with a short list of so-called "dangerous" topics like "terrorism", and then decided that if anyone reported one of his messages, it was *probably* bad. On the other hand, I have a very western name and nobody reports anything I post, so either the report actually made it to a human being, or the algorithm simply decided it was probably fine.

Of course, the algorithm was wrong about my friend's message. But Facebook doesn't care. I'm sure a bunch of self-important programmers are just itching to tell me we can't deal with all the edge-cases in a commercial algorithm because it's infeasible to account for all of them. What I want to know is, have any of these engineers ever thought about **who the edge-cases are?** Have they ever thought about the kind of people who can't produce birth certificates, or don't have a driver's license, or have strange names [that don't map to unicode properly](https://modelviewculture.com/pieces/i-can-text-you-a-pile-of-poo-but-i-cant-write-my-name) because they aren't western enough?

Poor people. Minorities. Immigrants. Disabled people. All these people they claim to care about, all this talk of diversity and equal opportunity and inclusive policies, and they're building algorithms that *by their very nature* will exclude those less fortunate than them. Facebook's algorithm probably doesn't even know that my friend is asian, yet it's still discriminating against him. Do you know who can follow all those rules and assumptions they make about normal people? Rich people. White people. Privileged people. These algorithms benefit those who don't need help, and disproportionately punish those who don't need any more problems.

What's truly terrifying is that [Silicon Valley wants to run the world](https://www.nytimes.com/2017/10/18/upshot/taxibots-sensors-and-self-driving-shuttles-a-glimpse-at-an-internet-city-in-toronto.html), and it wants to automate everything using a bunch of inherently flawed algorithms. Algorithms that might be *impossible* to perfect, given the almost unlimited number of edge-cases that reality can come up with. In fact, as I am writing this article, Chrome doesn't recognize "outlier" as a word, even though [Google itself does](https://www.google.com/search?q=outlier).

Of course, despite this, Facebook already built an algorithm that tries to detect "toxicity" and silences "unacceptable" opinions. Even if they could build a perfect algorithm for detecting "bad speech", do these companies really think forcibly restricting free speech will accomplish anything other than improving their own self-image? A deeply cynical part of me thinks the only thing these companies actually care about is looking good. A slightly more optimistic part of me thinks a bunch of well-meaning engineers are simply being stupid.

You can't change someone's mind by punching them in the face. Punching people in the face may shut them up, but it does not change their opinion. It doesn't *fix* anything. *Talking* to them does. I'm tired of this industry hiding problems behind shiny exteriors instead of fixing them. That's what used car salesmen do, not engineers. Programming has devolved into an art of deceit, where coders hide behind pretty animations and huge frameworks that sweep all their problems under the rug, while simultaneously screwing over the people who were supposed to benefit from an "egalitarian" industry that seems less and less egalitarian by the day.

Either silicon valley needs to start dealing with people that don't fit in neat little boxes, or it will no longer be able to push humanity forward. If we're going to move forward as a species, we have to do it **together**. Launching a bunch of rich people into space doesn't accomplish anything. Curing cancer for rich people doesn't accomplish anything. Inventing immortality for rich people doesn't accomplish anything. If we're going to push humanity forward, we have to push *everyone* forward, and that means dealing with all 7 billion outliers.

I hope silicon valley doesn't drag us back to the feudal age, but I'm beginning to think [it already has](https://medium.com/@ebonstorm/feudalism-and-the-algorithmic-economy-62d6c5d90646).
