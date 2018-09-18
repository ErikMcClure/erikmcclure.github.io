+++
categories = ["blog"]
date = "2018-09-18T11:03:49-07:00"
draft = true
title = "Software Engineering Is Bad, But It's Not That Bad"

+++
I've been writing code for over 12 years, and for a while now I've been pretty disgusted by the sorry state of programming. This is why I feel a deep kinship with Nikita Prokopov's article, [Software Disenchantment](http://tonsky.me/blog/disenchantment/ "Software Disenchantment"). It captures the intense feeling of frustration I have with the software industry as a whole and the inability of modern programmers to write anything even remotely resembling efficient systems.

**Unfortunately, most of it is wrong.**

While I wholeheartedly agree with what Nikita is saying, I am afraid that he doesn't seem to understand how modern computers work. This is unfortunate, because a misinformed article like this weakens both our positions and simply makes it more difficult to convince people that software bloat is a real problem. Most of the time, the frustrations he is expressing are valid, but his actual reasoning behind them is misdirected. So, in this article, I'm going to go down each of his points and either agree with it, or correct it.

1. **Smooth Scroll**

One of my hobbies is game development, and I even built a graphics engine once (for some reason). Drawing literally anything at 4K resolution at 60 FPS is _insanely hard_. Most games struggle to render at 4K 60 FPS with hugely powerful GPUs, and the 2D games that can render at that speed are usually graphically simplistic, in that they can have lots of fancy drawings, but drawing 200 fancy images on the screen with simplistic blending is not very difficult. Web page renderers don't have it this easy, because HTML has extremely specific composition rules. There is also another crucial difference between a 4K video, a video game, and a webpage: **text**. High quality anti-aliased and sometimes sub-pixel hinted text at 10 different sizes on different colored backgrounds blended with different transparencies on all sorts of different buttons is just really goddamn hard to render. Games don't do any of that. They have simple UIs with one or two fonts that are often either pre-rendered or use signed distance fields to approximate them. A web browser is rendering arbitrary unicode text that could include emojis and god knows what else. This is _hard_, and getting it to run on a GPU is even harder. One of the most impressive pieces of technology that exists today is Firefox's Servo engine, which actually manages to render most of this on a GPU and consequently 