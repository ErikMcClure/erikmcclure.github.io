+++
title = "Write Less Code  "
date = 2013-09-12T10:36:00Z
updated = 2013-09-12T10:36:56Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

{{%blockquote%}}*"Everything should be as simple as possible, but not simpler."* - Albert Einstein ([paraphrased](http://quoteinvestigator.com/2011/05/13/einstein-simple/#more-2363)){{%/blockquote%}}

The burgeoning complexity of software is perhaps one of the most persistent problems plaguing computer science. We have tried many, many ways of managing this complexity: inventing new languages, creating management systems, and enforcing coding styles. They have all proven to be nothing more than stopgaps. With the revelation that the NSA is utilizing the inherent complexity of software systems to sabotage our efforts at securing our systems, this problem has become even more urgent.

It's easy to solve a problem with a lot of code - it's hard to solve a problem with a little code. Since most businesses do not understand the advantage of having high quality software, the most popular method of managing complexity is not to reduce how much code is used, but to wrap it up in a self-contained library so we can ignore it, instead. The problem with this approach is that you haven't gotten rid of the code. It's still there, it's just easier to ignore.

Until it breaks, that is. After we have wrapped complex systems into easy-to-use APIs, we then build even more complex systems on top of them, creating networks of interconnected components, each one encapsulating it's own complex system. Instead of addressing this burgeoning complexity, we simply wrap it into *another* API and pave over it, much like one would pave over a landfill. When it finally breaks, we have to [extract a core sample](http://ptrthomas.files.wordpress.com/2006/06/jtrac-callstack1.png?w=630) just to get an idea of what might be going wrong.

One of the greatest contributions functional programming has made is its concept of a pure function with no side-effects. When a function has side effects, it's another edge on the graph of components where something could go wrong. Unfortunately, this isn't enough. If you simply replace a large complex system with a bunch of large complex functions that technically have no side-effects, it's still a large complex system.

Object-oriented programming tries to manage complexity by compartmentalizing things and (in theory) limiting how they interact with each other. This is designed to address spaghetti code that has a bunch of inter-dependent functions that are impossible to effectively debug. The OO paradigm has the right idea: "an object should do exactly one thing, and do it well", but suffers from over-application. OO programming was designed to manage *very large complex objects*, not concepts that don't need Get/Set functions for every class member. Writing these functions *just in case* you need them in the future is a case of future-proofing, which should almost always be avoided. If there's an elegant, simple way to express a solution that only works given the constraints you currently have, ***USE IT***. We don't need any more [leaning towers of inheritance](http://limejuice.fastmail.fm/expand-all-example.png).

The number of bugs in a software program is proportional to how much code you write, and that doesn't include cheap tricks like the ternary operator or importing a bunch of huge libraries. The logic is still there, the complexity is still there, and it will still break in mysterious ways. In order to reduce the complexity in our software, it has to do *less stuff*. The less code you write, the harder it is for the NSA to sabotage your program without anyone noticing. The less code you write, the fewer points of failure your program will have. The less code you write, the less stuff will break. We cannot hide from complexity any longer; we must actively reduce it. We have to begin excavating the landfill instead of paving over it.
