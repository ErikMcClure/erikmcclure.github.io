+++
categories = ["blog"]
date = "2018-02-10T18:01:40+00:00"
draft = true
title = "Software Optimizes to Single Points of Failure"

+++
Whenever people talk about removing single points of failure, most of the suggestions involve "distributed systems" that are resilient to hardware failures. For software, we've invented code signing and smart contracts via blockchain to ensure the code we're running is what we expected to run.

But none of these technologies can prevent a bug from taking down the entire system.

A lot of people point to Google being a single point of failure. They are only partially correct, because Google's _hardware_ is distributed and extremely redundant. No single piece of hardware in a Google Data center failing can take down the entire data center. You could probably nuke the _entire data center_ and most Google services could fall back to another data center. In fact, Google has developed software canaries to catch bugs from propagating too far into production in an attempt to address the problem of their software being a single point of failure.

But something **did** take down the entirety of Google Compute once. It was a software bug [in the canary itself](https://status.cloud.google.com/incident/compute/16007). Of course, **all** the canaries were running the same software, so all of them had the same bug, and none of them could catch the configuration bug that was being propagated to all of their routers.

By creating a software canary, Google had simply shifted the single point of failure to its canary software. It was much harder for it to fail, but it was still a single point of failure, so when it did fail, it took down the entire system.

We've put a lot of work into trying to reduce the amount of bugs in mission critical systems, going so far as to try to create [provably correct software](https://sel4.systems/). The problem is that no system can prove that it is free of _design-flaws_, which occur when the software operates correctly, but does something nobody actually wanted it to do. All of our code-signing and trusted computing initiatives do is make it very difficult for someone to sneak bad code into a widely used library. None of them, however, remove the single point of failure. Should the NSA ever succeed in sneaking in a backdoor to a widely used open source library, which gets passed all the code reviews and code signing and every other check we have, it will propagate to everything. It will be game over.

A very well guarded single point of failure is still a single point of failure, no matter how remote the chances are of it actually failing.