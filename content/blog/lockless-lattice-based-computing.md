+++
blogimport = true
categories = ["blog"]
date = "2012-10-15T14:24:00Z"
title = "Lockless Lattice-Based Computing"
updated = "2012-10-15T14:24:16.000+00:00"
comments = [ 4541202557533023376, 4721357949272230233 ]
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
The **[Flow Programming Language](http://www.flowlang.net/p/solving-multicore-dilemma.html)** was introduced as a form of *lattice-based computing*{{<sup>}}<a href="#f1">1</a>{{</sup>}}. This method of thinking about code structure was motivated by the need to make implicit parallelization of code possible at compile-time. Despite this, it has fascinated me for a number of reasons that extend well beyond simple multithreading applications. Most intriguing is its complete elimination of memory management{{<sup>}}<a href="#f2">2</a>{{</sup>}}. A lattice-based programming language doesn't have manual memory allocation or garbage collection, because it knows when and where memory is needed throughout the entire program. This is particularly exciting to me, because for the longest time I've had to use C or C++ simply because they are the only high-performance languages in existence that *don't* have garbage collection, which can be catastrophic in real-time scenarios, like games.

Naturally, this new methodology is not without its flaws. In particular, it currently has no way of addressing the issue of what to do when one variable depends on the values of multiple other variables, which can give rise to race conditions.

{{<img style="text-align:center;" src="/img/Flow-DAG.png" alt="Flow DAG" >}}

In the above diagram, both {{<code>}}e{{</code>}} and {{<code>}}f{{</code>}} depend on multiple previous results. This is a potential race condition, and the only current proposed solution is using traditional locks, which would incur contention issues{{<sup>}}<a href="#f3">3</a>{{</sup>}}. This, however, is not actually necessary. 

Instead of using an ugly lock, we can instead pair each variable up with a **parameter counter**. This counter keeps track of the number of dependent variables that have yet to be calculated. When a variable is resolved, it atomically decrements the parameter counter for all variables that are immediately dependent on it. If this atomic decrement results in a value of exactly 0, the thread can take one of 3 options:

 1. Queue a task to the thread manager to execute the variable's code path at some point in the future.
 1. Add the variable to its own execution queue and evaluate it later.
 1. Evaluate the variable and its execution path immediately (only valid if there are no other possible paths of execution)

If a thread reaches the a situation where all variables dependent on the last value it evaluated have nonzero parameter counts, then the thread simply "dies" and is put back into the thread scheduler to take on another task. This solution takes advantage of the fact that any implicit parallelization solution will require a robust method of rapidly creating and destroying threads (most likely through some kind of task system). Consequently, by concentrating all potential race conditions and contention inside the thread scheduler itself, we can simply implement it with a lockless algorithm and eliminate every single race condition in the entire program.

It's important to note that while the thread scheduler may be implemented with a lock-free (or preferably wait-free) algorithm, the program itself does not magically become lock-free, simply because it can always end up being reduced to a value that everything else depends on which then locks up and freezes everything. Despite this, using parameter counters provides an elegant way of resolving race-conditions in lattice-based programming languages.

This same method can be used to construct various intriguing multi-threading scenarios, such as attempting to access a GPU. Calling a GPU {{<code>}}DrawPrimitive{{</code>}} method cannot be done from multiple threads simultaneously. Once again, we could solve this in a lattice-based programming language by simply introducing locks, but *I hate locks*. Surely there is a more elegant approach? One method would be to simply construct an algorithm that builds a queue of processed primitives that is then pumped into the GPU through a single thread. This hints at the idea that so long as a non-thread-safe function is isolated to one single thread of execution inside a program, we can guarantee it will never be called at the same time at compile time. In other words, it can only appear once per-level in the DAG.

Of course, what if we wanted to render everything in a specific order? We introduce a secondary dependency in the draw call, such that it is dependent both on the primitive processing results, and a dummy variable that is resolved only when the drawing call directly before it is completed. If the primitive processing finishes before the previous drawing call is completed, the thread simply dies. When the previous drawing call is finished, that thread decrements the variable's parameter count to 0 and, because there are no other avenues of execution, simply carries on finishing the drawing call for the now dead thread. If, however, the previous drawing call is completed before the next primitive can be processed, the **previous** thread now dies, and when the primitive drawing is completed, that thread now carries on the drawing calls.

<img src="http://imageshack.us/a/img11/8155/threadprogression2.png" alt="Threads of Execution" />

What is interesting is that this very closely mirrors my current implementation in C++ for achieving the exact same task in a real-world scenario, except it is potentially even more efficient due to its ability to entirely avoid dynamic memory allocation. This is very exciting news for high performance programmers, and supports the idea of Lattice-Based Programming becoming a valuable tool in the near future.

<span style="font-size:80%">
<br/><sup><a name="f1">1</a></sup> I'd much rather call it "Lattice-Based Programming", since it's a way of thinking about code and not just computing.
<br/><sup><a name="f2">2</a></sup> I would link to this if the original post had anchors to link to.
<br/><sup><a name="f3">3</a></sup> Also, I happen to have a deep-seated, irrational hatred of locks.
</span>
