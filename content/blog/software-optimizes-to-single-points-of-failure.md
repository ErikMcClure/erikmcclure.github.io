+++
categories = ["blog"]
date = "2018-02-10T18:01:40Z"
title = "Software Optimizes to Single Points of Failure"

+++
Whenever people talk about removing single points of failure, most of the suggestions involve "distributed systems" that are resilient to hardware failures. For software, we've invented code signing and smart contracts via blockchain to ensure the code we're running is what we expected to run.

But none of these technologies can prevent a bug from taking down the entire system.

A lot of people point to Google being a single point of failure. They are only partially correct, because Google's _hardware_ is distributed and extremely redundant. No single piece of hardware in a Google Data center failing can take down the entire data center. You could probably nuke the _entire data center_ and most Google services could fall back to another data center. In fact, Google has developed software canaries to catch bugs from propagating too far into production in an attempt to address the problem of their software being a single point of failure.

But something **did** take down the entirety of Google Compute once. It was a software bug [in the canary itself](https://status.cloud.google.com/incident/compute/16007). Of course, **all** the canaries were running the same software, so all of them had the same bug, and none of them could catch the configuration bug that was being propagated to all of their routers.

By creating a software canary, Google had simply shifted the single point of failure to its canary software. It was much harder for it to fail, but it was still a single point of failure, so when it did fail, it took down the entire system.

We've put a lot of work into trying to reduce the amount of bugs in mission critical systems, going so far as to try to create [provably correct software](https://sel4.systems/). The problem is that no system can prove that it is free of _design-flaws_, which occur when the software operates correctly, but does something nobody actually wanted it to do. All of our code-signing and trusted computing initiatives do is make it very difficult for someone to sneak bad code into a widely used library. None of them, however, remove the single point of failure. Should the NSA ever succeed in sneaking in a backdoor to a widely used open source library, it will propagate to everything.

A very well guarded single point of failure is still a single point of failure, no matter how remote the chances of it actually failing. [Tom Scott has an excellent video](https://www.youtube.com/watch?v=y4GB_NDU43Q) about how a trusted engineer at Google that is allowed to bypass all their security checks could go rogue and remove all the password checks on everything and it would be incredibly hard to stop them.

Physical infrastructure is much more resilient to these kinds of problems, because even if every piece of infrastructure has the same problem, you still have to _physically get to it_ in order to exploit the problem. This makes it very hard for anyone to simultaneously sabotage any country's offline infrastructure without an incredible amount of work. Software, however, lets us access everything from everywhere. The internet removes physical access as a last resort.

Of course, this is not an insurmountable problem, but it is deceptively difficult to overcome. For example, let's say we have a bunch of drones we're controlling. To avoid one bug from taking all of them out at once, half of them run one flying program and the other run a completely different flying program, developed independently. Unfortunately, both of these programs rely on the same library that reads the gyroscope data. If that library has a bug, the entire swarm will crash into a mountain. Having the swarm calculate the data for each other and compare results doesn't help, because _everyone gets the wrong result_. The software logic itself is wrong.

The reason this is so insidious is that it runs counter to sane software development practices. To minimize bugs, we minimize complexity, which means writing the least amount of code possible, which inadvertently optimizes to a single point of failure. We re-use libraries and share code. We deliberately try to solve problems exactly once and re-use this code everywhere in our program. This a _good thing_ because it means any bugs we fix propagate everywhere else, but this comes at the cost of propagating any bugs we _introduce_.

Soon, our world will be consumed by automation, one way or another. [Cory Doctorow suggests](https://boingboing.net/2012/01/10/lockdown.html) that hardware should only run software the user trusts, but what if I end up trusting buggy software? If all our self-driving cars run the same software, what happens when it has a bug? Even worse, what if all the different self-driving car companies have their own software, custom built by highly paid engineers... that all use OpenSSL to securely download updates?

[What if OpenSSL has a bug?](https://en.wikipedia.org/wiki/Heartbleed)

It's not clear what can be done about this. Obviously we shouldn't go around introducing unnecessary complexity that creates even more bugs, but at the same time we shouldn't delude ourselves into thinking our distributed systems have no single point of failure. They may be robust to hardware failures, but the software they run on will continue to be a single point of failure for the foreseeable future.