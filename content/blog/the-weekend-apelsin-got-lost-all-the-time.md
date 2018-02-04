+++
title = "The Weekend Apelsin Got Lost All The Time"
date = 2012-11-13T23:24:00Z
updated = 2012-11-13T23:24:45Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

So on thursday morning my friend [apelsin](https://soundcloud.com/apelsin) mentioned a hackathon at his college, San Jose University. I expressed interest but pointed out that I live 800 miles away. So he bought me a ticket on the last plane flying to San Francisco that night. 10 hours after he had mentioned this hackathon with me, I was riding on a train to San Jose University (after missing one train and getting off on the wrong stop).

The hackathon had prizes - $3100, $1500, and $600 for 1{{<sup>}}st{{</sup>}}, 2{{<sup>}}nd{{</sup>}}, and 3{{<sup>}}rd{{</sup>}}, respectively. The goal was to build, in 24 hours, a AI for a game. The game was a 7x7 board with empty tiles on it. To win a round, you must construct a continuous path from one side to the other. Player 1 must construct a path connecting the left and right sides, while player 2 must construct a path from top to bottom. The squares are grouped into randomized sets, which are then run through over and over, and each time a set is made available, each bot places a bid on the set. The highest bid gets to pick any square from that set and claim it. You had 98 credits to bid with (7*7*2 = 98) and there was no way to get more.

I was able to build an atemporal pathfinding algorithm that constructed a 7-turn path by figuring out in advance which squares would be available which turn. To prevent difficult edge cases I simplified it so it would always try to do it in 7 turns and simply bid 14 credits each time. My friend helped with debugging and brainstorming ideas, as well as setting everything up, and we had to keep correcting each other on how the game actually worked, but the entire time I was also learning how to use a mac, since my only laptop was a piece of shit and he only had macs. 

We were also trying to use Codelite in the hopes that it would be better than Xcode, but that ended up being a disaster with constant crashes and failed debuggers. As a result, I learned two things from this hackathon: I fucking hate macs, and Codelite is a horrible, unstable IDE you should never use. 

I was only able to stabilize our algorithm with 40 minute left in the competition. I attempted to implement an aggressive analysis algorithm that would detect a potential win condition for the opponent and block it, but it never worked. Had I instead extended the algorithm to simply look for alternative paths longer than 7 turns, we probably would have won, but I didn't realize that until after the competition. Even though we only had a very basic algorithm, our pathfinding was extremely strong and as a result we got all the way to 4th place. Unfortunately, that wasn't enough for a cash prize, so we weren't able to recoup the plane ticket cost.

So then we took a train back up to his house in San Francisco, went out for dinner, and did more music stuff with his crazy studio monitor setup. The next day involved fisherman's wharf, candy, being lost, and getting to play around on a hardware synth. Then I went to bed and today I got flown back to Seattle.

All because my friend looked at a hackathon advertisement on Thursday morning.
