+++
categories = ["blog"]
date = "2018-09-18T11:03:49-07:00"
draft = true
title = "Software Engineering Is Bad, But It's Not That Bad"

+++
I've been writing code for over 12 years, and for a while now I've been pretty disgusted by the sorry state of programming. This is why I feel a deep kinship with Nikita Prokopov's article, [Software Disenchantment](http://tonsky.me/blog/disenchantment/ "Software Disenchantment"). It captures the intense feeling of frustration I have with the software industry as a whole and the inability of modern programmers to write anything even remotely resembling efficient systems.

**Unfortunately, most of it is wrong.**

While I wholeheartedly agree with what Nikita is saying, I am afraid that he doesn't seem to understand how modern computers work. This is unfortunate, because a misinformed article like this weakens both our positions and simply makes it more difficult to convince people that software bloat is a real problem. Most of the time, the frustrations he is expressing are valid, but his actual reasoning behind them is misdirected. So, in this article, I'm going to write counterpoints to some of the more problematic claims.

1. **Smooth Scroll**

One of my hobbies is game development, and I even built a graphics engine once (for some reason). Doing anything at 4K resolution at 60 FPS on a laptop is _insanely hard_. Most games struggle to render at 4K 60 FPS with hugely powerful GPUs, and the 2D games that can render at that speed are usually graphically simplistic, in that they can have lots of fancy drawings, but drawing 200 fancy images on the screen with simplistic blending is not very difficult. A video is just a single 4K image rendered 60 times a second, which is even easier. Web page renderers can't do that, because HTML has extremely specific composition rules that will break a naïve graphics pipeline. There is also another crucial difference between a 4K video, a video game, and a webpage: **text**.

High quality anti-aliased and sometimes sub-pixel hinted text at 10 different sizes on different colored backgrounds blended with different transparencies on all sorts of different buttons is just really goddamn hard to render. Games don't do any of that. They have simple UIs with one or two fonts that are often either pre-rendered or use signed distance fields to approximate them. A web browser is rendering arbitrary unicode text that could include emojis and god knows what else. Sometimes it's even doing transformation operations _in realtime_ on an SVG vector image. This is _hard_, and getting it to run on a GPU is even harder. One of the most impressive pieces of technology that exists today is Firefox's [WebRender](https://hacks.mozilla.org/2017/10/the-whole-web-at-maximum-fps-how-webrender-gets-rid-of-jank/), which actually manages to render most of this on a GPU without intermediate backtracking to the CPU and consequently can actually serve most webpages at a smooth 60 FPS. This is basically as fast as you could possibly make any of this. Of all the things to complain about, this is one of the weirdest, because Firefox's WebRender is actually one of the most impressive engineering feats in modern web development, and is equivalent to a well-tuned car engine running at 98% of theoretical capacity.

I think the real issue here, and perhaps what Nikita was getting at, is simply that the _design_ of modern webpages is so bloated that the web browsers just can't keep up. The poor browsers are getting inundated with <div> trees the size of Mount Everest and 10 megabytes worth of useless javascript bootstrapping ad campaigns that load embed entire miniature videos into your webpage. However, none of these aspects have anything to do with the resolution of the webpage or how fast it's rendering. The whole page is going to be slow if it's loading a bunch of useless crap no matter what GPU you have. Inbox taking 13 seconds to load anything is completely unacceptable, but animating anything other than a white box in HTML is far more expensive than you think.

2. **Latency**

Latency has to be one of the least understood values in computer electronics today. It is true that many text editors have abysmal response times caused by terrible code, but it's a lot easier to screw this up than a lot of people seem to realize. 