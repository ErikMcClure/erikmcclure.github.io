+++
title = "Posted While On A Bus"
date = 2009-11-25T13:46:00Z
updated = 2011-01-22T04:30:41Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Accessing the internet while I'm riding the bus home has to be one of the most awesome things I've done in a while. A pity the UW security is too ridiculous for me to tunnel through with hamachi on my router. But no worries, I'm getting the hell out of that shithole in less then 3 weeks, and then I won't have to worry about taking my clothes back and forth and back and forth... just my laptop.

Whispers are working except the Router plugin for Raknet is apparently not actually supported... but it turns out that it's rather unnecessary except for insane peer-to-peer connections anyway, so I replaced it with an RPC that's working quite nicely. Now I am programming a PHP serverlist that will double as a facilitator for a NAT punchthrough technique, which should remove the issue of we-have-to-use-hamachi-to-connect. Then its off to the most difficult networking task in the entire project - Physics interpolation. My task will be to both get a physics object to update itself over the network in an efficient manner, and I will require a networking interpolation hack in box2D in order to make it work in the first place, which of course must be optimized to ridiculousness. Luckily i think there's a way to put in a negative step into the interpolation function to get it to interpolate backwards, which would solve my security issue, although not the collision problem. I'd have to basically discard all collisions for the reversal. I'm really not sure how to get that to work, especially since I still need to figure out how and which collisions to disregard for the interpolation forward.

Once that works the only remaining physics problems to solve are 1. how to stagger the update packets based on proximity to an active player and 2. how to interpolate complex animated objects. The latter will be done  whenever i get around to having complex animated objects, but the former will have to be the result of ongoing optimization and fine tuning.

In other news, I have invented a method of document reconstruction that would allow art programs to recover all information from a drawing-in-progress even in extreme circumstances, such as power outages. Unfortunately while this isn't that difficult to implement, it requires a subtle feature that is implemented from the ground-up, so i wasn't able to code a proof of concept in paint.net :C
