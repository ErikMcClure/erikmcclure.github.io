+++
blogimport = true
categories = ["blog"]
date = "2010-08-06T04:18:00Z"
title = "Physics Networking"
updated = "2011-01-22T03:54:20.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I'm still working on integrating physics into my game, but at some point here I am going to hit on that one major hurdle: syncing one physics environment with another that could be halfway across the globe. There are a number of ways to do this; some of them are bad, and some of them are absolutely terrible. 

If any of you have played Transformice, you will know what I mean by terrible. While I did decompile the game, I never bothered to examine their networking code, but I would speculate that they are simply mass-updating all the clients and not properly interpolating received packets. Consequently, when things get laggy, everyone doesn't just hop around, they completely clip through objects, even ones that they should never clip though *no matter what the other players are doing*.

The question is, how do you properly handle physics networking without flooding the network with unnecessary packets, especially when you are dealing with very large numbers of physics objects spread across a large map with many people interacting in complex ways?

In a proper setup, a client processes input from the user, and sends this to the server. While it's waiting for a response, it will interpolate the physics by guessing what all the other players are doing. If there are no other players this interpolation can, for the most part, be considered to be perfectly accurate. This will generally hold true if there are players that are far enough away from each other they can't directly or indirectly influence each other's interpolation.

Meanwhile, the server receives the input a little while later - say, 150 ms. In an ideal scenario, the entire physics world is rewound 150 ms and then re-simulated taking into account the player's action. The server then broadcasts new physics locations and the player's action to all other clients.

All the other clients now get these packets *another* 150 ms later. In an ideal scenario, all they would need to know is that the other player pressed a button 300 ms ago, rewind the simulation that much, then re-simulate to take it into account.

We are, obviously, not in an ideal scenario.

What exactly does this entail? In any interpolation function, speed is gained by making assumptions that introduce possible errors. This error margin grows as the player has more and more objects he might have interacted with. However, this also applies to each physics object against each *other* physics object, and in turn each other physics object from that. Hence, this boils down to the math equation: p = n{{<sup>}}n!{{</sup>}}. That is a worst case scenario. Best case scenario is that none of the physics objects interact with each other, so error margin p = n and is therefore linear. 

Hence, we now know that interaction with physics objects - or possibly, any sort of nonuniform physical force, like an explosion - is what creates the uncertainty problem. This is why the server always maintains its own physics world that is synced to everyone else, so that even if its not always *right*, its at least somewhat *consistent*. The question is, when do we need to send a physics packet, and when do we *not* need to send one?

If the player is falling through the air and jumps, and there is nothing he could possibly interact with, we can assume that any half-decent interpolation function will be almost perfect. Hence, we don't actually have to send any physics packets because the interpolation should be perfect. However, the more possible objects that are involved, the more uncertain things get and the more we need to send physics updates in case the interpolation functions get confused. In addition, we have to send packets for all affected *objects* as well. However, we only have to do this when the player gives input to the game. If the player's input does not change, then he is obeying the interpolation laws of everyone else and no physics update is needed, since the interpolation always assumes the player's input status has not changed. 

Hence, in a semi-ideal scenario, we only have to send physics packets for the player and any physics objects he might have collided with, and only when the player changes their input status. Right?

But wait, that works for the server, but not all the other clients. Those clients receive those physics packets 150 ms late, and have to interpolate *those* as well, introducing more uncertainty that cannot be eliminated. In addition, we aren't even in a semi-ideal scenario - the interpolation functions become more unreliable over time regardless of the player's input status. 

However, this uncertainty itself is still predictable. The more objects a player is potentially interacting with, the more uncertain those object states are. This grows exponentially when other players are in the mix because we simply cannot guess what they might do in those 150 ms.

Consequently, one technique would be to send physics updates for all potentially affected objects surrounding the player whenever the player changes their input *or interacts with another physics object*. This alone will only work when the player is completely alone, and even then require some intermediate packets for higher uncertainty. Hence, one can create an update pattern that looks something like this:

**Physics packet rapidity: (number of potential interacting physics objects) * ( (other players) * (number of their potential interacting objects) )**

More precise values could be attained by taking into account distance and understand exactly what and how the interpolation function is working. Building an interpolation function that is capable of rewinding and accurately re-simulating the small area around the player is obviously a very desirable course of action, because it removes the primary source of uncertainty and would allow for a lot more breathing room.

Either way, the general idea still stands - the closer your player is to a physics object, the more often that object (and the player) need to be updated. The closer your player is to another player, the update speed goes up exponentially. And always remember to send velocity data too!

I will make a less theoretical post on this after I've had an opportunity to do testing on what *really* works.
