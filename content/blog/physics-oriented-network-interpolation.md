+++
blogimport = true
categories = ["blog"]
date = "2009-11-10T11:04:00Z"
title = "Physics-oriented Network Interpolation"
updated = "2011-01-22T04:35:15.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
Syncing a game over a client server connection is not an easy task. It's actually extraordinarily difficult and is almost completely reliant on the quality of interpolation. Due to the nature of interpolation, it gets exponentially more inaccurate the more time is spent doing it. Therefore, a game designer should want to minimize the amount needed. This is not an easy task, but it usually involves using the server as a middleman to halve the interpolation time. The only issue with this is that the server's interpolation becomes reality, so while a client interpolation can be fairly inaccurate and simply corrected later on, the server's world is reality.

This has two consequences, one is fairly obvious: the server interpolation between the time the player hit the move button and their current location must be extremely accurate, while the client interpolation can be fairly sloppy. If however, the server is a dedicated server (or has a parallel physics world), then a small trick can be employed - the server's physics world need only be updated with every physics packet received, enabling an accurate physics simulation for a fraction of a second and eliminating a small amount of interpolation. An additional measure can be taken by utilizing a peer-to-peer connection between the clients and sending tiny packets of button notifications. These, if they happen to get to the client before the server packet does, can improve perceived responsiveness by giving the client a heads up on whether a player has fired something.

Another possible method of interpolation involves knowing where a shot should be and where it is currently in the view of a network player, and accelerating it until it reaches its intended destination. This is, however, problematic in terms of multiplayer shooter games because a shot quite often will hit another player or some object within the timespan of the ping and subsequently cause massive confusion on part of the player who thinks his shot is on the bottom of the screen rather then the top. In cases where accuracy is crucial, it is often best to simply have shots appear "out of nowhere" in front of the player that shot them, but ***play the shooting animation at the same time***. This visual feedback triggers an instinctual "oh its lag" response, instead of "where the hell did that shot come from." 

All of these methods are applied in anticipation of a perfect interpolation function, which is of course impossible. Hence, while we can mitigate most of the problems that exist even with a perfect interpolation problem, it comes down to  simply coding a better, faster interpolation function.
