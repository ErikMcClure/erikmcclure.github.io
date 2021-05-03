+++
categories = ["blog"]
date = "2021-05-01T23:04:00Z"
title = "C++ Constructors, Memory, and Lifetimes"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{<img src="/img/cpp_init_forest.gif" alt="C++ Initialization Hell" width="400">}}

What exactly happens when you write `{{<code cpp>}}Foo* foo = new Foo();{{</code>}}`? A lot is packed into this one statement, so lets try to break it down. First, this example is allocating new memory on the heap, but in order to understand everything that's going on, we're going to have to explain what it means to declare a variable *on the stack*. If you already have a good understanding of how the stack works, and how functions do cleanup before returning, feel free to skip to the [new statement](#new-statements).

### Stack Lifetimes
Describing the stack is very often glossed over in many other imperative languages, despite the fact that those languages still have one (functional languages are an entirely different level of weird). Let's start with something very simple:
{{<pre cpp>}}int foobar(int b)
{
  int a;
  a = b;
  return a;
}
{{</pre>}}
Here, we are declaring a function `foobar` that takes an `int` and returns an `int`. The first line of the function declares a variable `a` of type `int`. This is all well and good, but *where is the integer?*. On most modern platforms, `int` resolves to a 32-bit integer that takes up 4 bytes of space. We haven't allocated any memory yet, because no `new` statement happened and no `malloc()` was called. Where is the integer?

The answer is that the integer was allocated on the *stack*. If you aren't familiar with the [computer science data structure](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) of the same name, your program is given a chunk of memory by the operating system that is organized into a stack structure, hence the name. It's like a stack of plates - you can push items on top of the stack, or you can remove items from the top of the stack, but you can't remove things from the middle of the stack or all the plates will come crashing down. So if we push something on top of the stack, we're stuck with it until we get rid of everything on top of it.

When we called our function, the parameter `int b` was pushed on to the stack. Parameters take up memory, so on to the stack they go. Hence, before we ever reach the statement `int a`, 4 bytes of memory were already pushed onto our stack. Here's what our stack looks like at the beginning of the function if we call it with the number `90` (assuming little-endian):

{{<img src="/img/stack1.svg" alt="Stack for b" width="450">}}

`int a` tells the compiler to push another 4 bytes of memory on to the stack, but it has no initial value, so the contents are undefined:

{{<img src="/img/stack2.svg" alt="Stack for a and b" width="450">}}

`a = b` assigns b to a, so now our stack looks like this:
  
{{<img src="/img/stack3.svg" alt="Stack for initialized a and b" width="450">}}
    
Finally, `return a` tells the compiler to evaluate the return expression (which in our case is just `a` so there's nothing to evaluate), then copy the result into a chunk of memory we reserved ahead of time for the return value. Some programmers may assume the function returns *immediately* once the `return` statement is executed - after all, that's what `return` means, right? However, the reality is that the function still has to clean things up before it can actually return. Specifically, we need to return our stack to the state it was before the function was called by removing everything we pushed on top of it **in reverse order**. So, after copying our return value `a`, our function pops the top of the stack off, which is the last thing we pushed. In our case, that's `int a`, so we pop it off the stack. Our stack now looks like this:
  
{{<img src="/img/stack1.svg" alt="Stack without a" width="450">}}

The moment from which `int a` was pushed onto the stack to the moment it was popped off the stack is called the **lifetime** of `int a`. In this case, `int a` has a lifetime of the entire function. After the function returns, our caller has to pop off `int b`, the parameter we called the function with. Now our stack is empty, and the **lifetime** of `int b` is longer than the **lifetime** of `int a`, because it was pushed first (before the function was called) and popped afterwards (after the function returned). C++ builds it's entire concept of constructors and destructors on this concept of lifetimes, and they can get very complicated, but for now, we'll focus only on stack lifetimes.

Let's take a look at a more complex example:
{{<pre cpp>}}int foobar(int b)
{
  int a;
  
  {
    int x;
    x = 3;
    
    {
      int z;
      int max;
      
      max = 999;
      z = x + b;
      
      if(z > max)
      {
        return z - max;
      }
      
      x = x + z;
    }
    
    // a = z; // COMPILER ERROR!
    
    {
      int ten = 10;
      a = x + ten;
    }
  } 
  
  return a;
}
{{</pre>}}
Let's look at the lifetimes of all our parameters and variables in this function. First, before calling the function, we push `int b` on to the stack with the value of whatever we're calling the function with - say, `900`. Then, we call the function, which immediately pushes `int a` on to the stack. Then, we *enter a new block* using the character `{`, which does not consume any memory, but instead acts as a marker for the compiler - we'll see what it's used for later. Then, we push `int x` on to the stack. We now have 3 integers on the stack. We set `int x` to `3`, but `int a` is still undefined. Then, we *enter another new block*. Nothing interesting has happened yet. We then push both `int z` and `int max` on to the stack. Then we assign `999` to `int max` and assign `int z` the value `x + b` - if we passed in `900`, this means `z` is now equal to `903`, which is less than the value of `int max` (`999`), so we skip the if statement for now. Then we assign `x` to `x + z`, which will be `906`. 

Now things get interesting. Our topmost block **ends** with a `}` character. This tells the compiler to *pop all variables declared inside that block*. We pushed `int z` on to the stack inside this block, so it's gone now. We cannot refer to `int z` anymore, and doing so will be a compiler error. `int z` is said to have *gone out of scope*. However, we also pushed `int max` on to the stack, and we pushed it after `int z`. This means that the compiler will **first pop `int max` off the stack**, and only afterwards will it then pop `int z` off the stack. The order in which this happens will be critical for understanding how lifetimes work with constructors and destructors, so keep it in mind.

Then, we enter another new scope. This new scope is still inside the first scope we created that contains `int x`, so we can still access `x`. We define `int ten` and initialize it with `10`. Then we set `int a` equal to `x + ten`, which will be `916`. Then, our scope ends, and `int ten` goes out of scope, being popped off the stack. Immediately afterwards, we reach the end of our first scope, and `int x` is popped off the stack.

Finally, we reach `return a`, which copies `a` to our return value memory segment, pops `int a`, and returns to our caller, who then pops `int b`. That's what happens when we pass in `900`, but what happens if we pass in `9000`?

Everything is the same until we reach the `if` statement, whose condition is now satisfied, which results in the function terminating early and returning `z - max`. What happens to the stack?

When we reach `return z - max`, the compiler evaluates the statement and copies the result (`8004`) out. Then it starts popping everything off the stack (once again, in the reverse order that things were pushed). The last thing we pushed on to the stack was `int max`, so it gets popped first. Then `int z` is popped. Then `int x` is popped. Then `int a` is popped, the function returns, and finally `int b` is popped by the caller. This behavior is critical to how C++ uses lifetimes to implement things like smart pointers and automatic memory management. Rust actually uses a similar concept, but it uses it for a lot more than C++ does.

### `new` Statements

Okay, now we know how lifetimes work and where variables live when they aren't allocated, but what happens when you *do* allocate memory? What's going on with the `new` statement? To look at this, let's use a simplified example:
{{<pre cpp>}}int* foo = new int();
{{</pre>}}
Here we have allocated a pointer to an integer on the stack (which will be 8 bytes if you're on a 64-bit system), and assigned the result of `new int()` to it. What happens when we call `new int()`? In C++, the `new` operator is an extension of `malloc()` from C. This means it allocates memory from the *heap*. When you allocate memory on the heap, it never goes out of scope. This is what most programmers are familiar with in other languages, except that most other languages handle figuring out when to deallocate it and C++ forces you to delete it yourself. Memory allocated on the heap is just there, floating around, forever, or until you deallocate it. So this function has a memory leak:
{{<pre cpp>}}int bar(int b)
{
  int* a = new int();
  *a = b;
  return *a;
}
{{</pre>}}
This is the same as our [first example](#stack-lifetimes), except now we allocate `a` on the heap instead of the stack. So, it never goes out of scope. It's just there, sitting in memory, forever, until the process is terminated. The `new` operator looks at the type we passed it (which is `int` in this case) and calls `malloc` for us with the appropriate number of bytes. Because `int` has no constructors or destructors, it's actually equivelent to this:
{{<pre cpp>}}int bar(int b)
{
  int* a = (int*)malloc(sizeof(int));
  *a = b;
  return *a;
}
{{</pre>}}
Now, people who are familiar with C will recognize that any call to `malloc` should come with a call to `free`, so how do we do that in C++? We use `delete`:
{{<pre cpp>}}int bar(int b)
{
  int* a = new int();
  *a = b;
  int r = *a;
  delete a;
  return r;
}
{{</pre>}}
**IMPORTANT:** Never mix `new` and `free` or `malloc` and `delete`. The `new`/`delete` operators can use a different allocator than `malloc`/`free`, so things will violently explode if you treat them as interchangeable. Always `free` something from `malloc` and always `delete` something created with `new`.

Now we aren't leaking memory, but we also can't do `return *a` anymore, because it's impossible for us to do the necessary cleanup. If we were allocating on the stack, C++ would clean up our variable for us after the `return` statement, but we can't put anything after the return statement, so there's no way to tell C++ to copy the value of `*a` and *then* manually delete `a` without introducing a new variable `r`. Of course, if we could run arbitrary code when our variable went out of scope, we could solve this problem! This sounds like a job for constructors and destructors!

### Constructors and `delete`

Okay, let's put everything together and return to our original statement in a more complete example:
{{<pre cpp>}}struct Foo
{
  // Constructor for Foo
  Foo(int b)
  {
    a = b;
  }
  // Empty Destructor for Foo
  ~Foo() {}
  
  int a;
};

int bar(int b)
{
  // Create
  Foo* foo = new Foo(b);
  int a = foo->a;
  // Destroy
  delete foo;
  return a; // Still can't return foo->a
}
{{</pre>}}
In this code, we still haven't solved the return problem, but we are now using constructors and destructors, so let's walk through what happens. First, `new` allocates memory on the heap for your type. `Foo` contains a 32-bit integer, so that's 4 bytes. Then, *after* the memory is allocated, `new` automatically calls the *constructor* that matches whatever parameters you pass to the type. Your constructor doesn't need to allocate any memory to contain your type, since `new` already did this for you. Then, this pointer is assigned to `foo`. Then we delete `foo`, which **calls the destructor first** (which does nothing), and *then* deallocates the memory. If you don't pass any parameters when calling `new Type()`, or you are creating an array, C++ will simply call the default constructor (a constructor that takes no parameters). This is all equivelent to:
{{<pre cpp>}}int bar(int b)
{
  // Create
  Foo* foo = (Foo*)malloc(sizeof(Foo));
  new (foo) Foo(b); // Special new syntax that ONLY calls the constructor function (this is how you manually call constructors in C++)
  int a = foo->a; 
  // Destroy
  foo->~Foo(); // We can, however, call the destructor function directly
  free(foo);
  
  return a; // Still can't return foo->a
}
{{</pre>}}
This uses a special new syntax that doesn't allocate anything and simply lets us call the constructor function directly on our already allocated memory. This is what the `new` operator is doing for you under the hood. We then call the destructor manually (which you *can* do) and free our memory. Of course, this is all still useless, because we can't return the integer we allocated on the heap!

### Destructors and lifetimes

Now, the magical part of C++ is that constructors and destructors are run *when things are pushed or popped from the stack* {{<sup>}}<a href="#f1">[1]</a>{{</sup>}}. The fact that constructors and destructors respect variable lifetimes allows us to solve our problem of cleaning up a heap allocation upon returning from a function. Let's see how that works:
{{<pre cpp>}}struct Foo
{
  // Default constructor for Foo
  Foo()
  {
    a = new int();
  }
  // Destructor frees memory we allocated using delete
  ~Foo()
  {
    delete a;
  }
  
  int* a;
};

int bar(int b)
{
  Foo foo;
  *foo.a = b;
  return *foo.a; // Doesn't leak memory!
}
{{</pre>}}
How does this avoid leaking memory? Let's walk through what happens: First, we declare `Foo foo` on the stack, which pushes 4 bytes on to the stack, and then C++ calls our default constructor. Inside our default constructor, we use `new` to allocate a new integer and store it in `int* a`. Returning to our function, we then set our integer pointer `foo.a` to `b`. Then, we return the value stored in `foo.a` from the function{{<sup>}}<a href="#f2">[2]</a>{{</sup>}}. This copies the value out of `foo.a` first by dereferencing the pointer, and *then* C++ calls our destructor `~Foo` before `Foo foo` is popped off the stack. This destructor deletes `int* a`, ensuring we don't leak any memory. Then we pop off `int b` from the stack and the function returns. If we could somehow do this without constructors or destructors, it would look like this:
{{<pre cpp>}}int bar(int b)
{
  Foo foo;
  foo.a = new int();
  *foo.a = b;
  int retval = *foo.b;
  delete a;
  return retval;
}
{{</pre>}}
The ability to run a destructor when something goes out of scope is an incredibly important part of writing good C++ code, becuase when a function returns, *all* your variables go out of scope when the stack is popped. Thus, all cleanup that is done during destructors is gauranteed to run no matter when you return from a function. Destructors are gauranteed to run **even when you throw an exception!** This means that if you throw an exception that gets caught farther up in the program, you won't leak memory, because C++ ensures that everything on the stack is correctly destroyed when processing exception handling, so all destructors are run in the same order they normally are.

This is the core idea behind smart pointers - if a pointer is stored inside an object, and that object deletes the pointer in the destructor, then you will never leak the pointer because C++ ensures that the destructor will eventually get called when the object goes out of scope. Now, if implemented naively there is no way to pass the pointer into different functions, so the utility is limited, but C++11 introduced **move semantics** to help solve this issue. We'll talk about those later. For now, let's talk about different kinds of lifetimes and what they mean for when constructors and destructors are called.

### Static Lifetimes
Because any struct or class in C++ can have constructors or destructors, and you can put structs or classes anywhere in a C++ program, this means that there are rules for how to safely invoke constructors and destructors in all possible cases. These different possible lifetimes have different names. Global variables, or static variables inside classes, have what's called "static lifetime", which means their lifetime begins when the program starts and ends once the program exits. The exact order these constructors are called, however, is a bit tricky. Let's look at an example:
{{<pre cpp>}}struct Foo
{
  // Default constructor for Foo
  Foo()
  {
    a = new int();
  }
  // Destructor frees memory we allocated using delete
  ~Foo()
  {
    delete a;
  }
  
  int* a;
  static Foo instance;
};

static Foo GlobalFoo;

int main()
{
  *GlobalFoo.a = 3;
  *Foo::instance.a = *GlobalFoo.a;
  return *Foo::instance.a;
}
{{</pre>}}
When is `instance` constructed? When is `GlobalFoo` constructed? Can we safely assign to `GlobalFoo.a` immediately? The answer is that all static lifetimes are constructed **before your program even starts**, or more specifically, before `main()` is called. Thus, by the time your program has reached your entry point (`main()`), C++ gaurantees that all static lifetime objects have already been constructed. But what _order_ are they constructed in? This gets complicated. Basically, static variables are constructed in the order they are declared in a single `.cpp` file. However, the order these `.cpp` files are constructed in is undefined. So, you can have static variables that rely on each other inside a single `.cpp` file, but never between different files.

Likewise, all static lifetime objects get deconstructed _after_ your `main()` function returns, and once again, this order is random, although it _should_ be in the reverse order they were constructed in. Technically this should be respected even if an exception occurs, but because the compiler can assume the process will terminate immediately after an unhandled exception occurs, this is unreliable.

Static lifetimes still apply for shared libraries, and are constructed the moment the library is loaded into memory - that's `LoadLibrary` on Windows and `dlopen` on Linux. Most kernels provide a custom function that fires when the shared library is loaded or unloaded, and these functions fall outside of the C++ standard, so there's no gaurantee about whether the static constructors have actually been called when you're inside the `DllLoad`, but almost nobody actually needs to worry about those edge cases, so for any normal code, by the time any function in your DLL can be called by another program, you can rest assured all static and global variables have had their constructors called. Likewise, they are destructed when the shared library is unloaded from memory.

While we're here, there are a few gotchas in the previous example that junior programmers should know about. You'll notice that I did not write `{{<code cpp>}}static Foo* = new GlobalFoo();{{</code>}}` - **this will leak memory!**. In this case, C++ doesn't actually call the destructor because `Foo` doesn't have a static lifetime, **the pointer it's stored in does!**. So the _pointer_ will get it's constructor called before the program starts (which does nothing, because it's a primitive), and then the pointer will have it's destructor called after `main()` returns, which also does nothing, which means `Foo` never actually gets deconstructed or deallocated. Always remember that C++ is *extremely* picky about what you do. C++ won't magically extend Foo's lifetime to the lifetime of the pointer, it will instead do *exactly* what you told it to do, which is to declare a global pointer primitive.

Another thing to avoid is to not accidentally write `Foo::instance.a = GlobalFoo.a;`, because this doesn't copy the integer, it copies *the pointer* from `GlobalFoo` to `Foo::instance`. This is extremely bad, because now `Foo::instance` will leak it's pointer and instead try to free `GlobalFoo`'s pointer, which was already deleted by `GlobalFoo`, so the program will crash, but only AFTER successfully returning 3. In fact, it will crash outside of the `main()` function completely, which is going to look very weird if you don't know what's going on.

### Implicit Constructors and Temporary Lifetimes 
Lifetimes in C++ can get complicated, because they don't just apply to function blocks, but also function parameters, return values, and expressions. This means that, for example, if we are calling a function, and we construct a new object inside the function call, there is an implicit lifetime that exists for the duration of the function call, which is well-defined but very weird unless you're aware of exactly what's going on. Let's look at a simple example of a function call that constructs an object:
{{<pre cpp>}}class Foo
{
  // Implicit constructor for Foo
  Foo(int b)
  {
    a = b;
  }
  // Empty Destructor for Foo
  ~Foo() {}
  
  int a;
}

int get(Foo foo)
{
  return foo.a;
}

int main()
{
  return get(3);
}
{{</pre>}}
To understand what's going on here, we need to understand *implicit constructors*, which are a "feature" of C++ you never wanted but got anyway. In C++, all constructors that take exactly 1 argument are *implicit*, which means the compiler will attempt to use call them to satisfy a type transformation. In this case, we are trying to pass `3` into the `get()` function. `3` has the type `int`, but `get()` takes an argument of type `Foo`. Normally, this would just cause an error, because the types don't match. But because we have a constructor for `Foo` that takes an `int`, the compiler actually calls it for us, constructing an object of type `Foo` and passing it into the function! Here's what it looks like if we do this ourselves:
{{<pre cpp>}}int main()
{
  return get(Foo(3));
}
{{</pre>}}
C++ has "helpfully" inserted this constructor for us inside the function call. So, now that we know our `Foo` object is being constructed inside the function call, we can ask a different question: When does the constructor get called, exactly? When is it destructed? The answer is that all the expressions in your function call are evaluated first, from left-to-right. Our expression allocated a new temporary `Foo` object by pushing it onto the stack and then calling the constructor. However, do be aware that compilers [aren't always so great about respecting initialization order](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=51253) in function calls or other initialization lists. But, ostensibly, they're *supposed* to be evaluated from left-to-right.

So, once all expressions inside the parameteres have been evaluated, we then push the parameters on to the stack and copy the results of the expressions into them, allocate space on the stack for the return value, and then we enter the function. Our function executes, copies a return value into the space we reserved, finishes cleaning up, and returns. Then we do something with the return value and pop our parameters off the stack. Finally, after all the function parameter boilerplate has been finished, our expressions go out of scope in reverse order. This means that destructors are called from right-to-left after the function returns. This is all roughly equivilent to doing this:
{{<pre cpp>}}int main()
{
  int b;
  {
    Foo a = Foo(3); // Construct Foo
    b = get(a); // Call function and copy result
  } // Deconstruct Foo
  return b;
}
{{</pre>}}
This same logic works for all expressions - if you construct a temporary object inside an expression, it exists for the duration of the expression. However, the exact order that C++ evaluates expressions is [extremely complicated and not always defined](https://en.cppreference.com/w/cpp/language/eval_order), so this is a bit harder to nail down. Generally speaking, an object gets constructed right before it's needed to evaluate the expression, and gets deconstructed afterwards. These are "temporary lifetimes", because the object only briefly exists inside the expression, and is deconstructed once the expression is evaluated. Because C++ expressions are not always ordered, you should not attempt to rely on any sort of constructor order for arbitrary expressions. As an example, we can inline our previous `get()` function:
{{<pre cpp>}}int main()
{
  return Foo(3).a;
}
{{</pre>}}
This will allocate a temporary object of type `Foo`, construct it with `3`, copy out the value from `a`, and then deconstruct the temporary object before the return statement is evaluated. For the most part, you can just assume your objects get constructed before the expression happens and get destructed after it happens - try not to rely on ordering more specific than that. The specific ordering rules are also changing in C++20 to make it more strict, which means how strict the ordering is will depend on what compiler you're using until everyone implements the standard properly.

For the record, if you don't want C++ "helpfully" turning your constructors into implicit ones, you can use the `explicit` keyword to disable that behavior:
{{<pre cpp>}}struct Foo
{
  explicit Foo(int b)
  {
    a = b;
  }
  ~Foo() {}
  
  int a;
};
{{</pre>}}
### Static Variables and Thread Local Lifetimes
Static variables inside a function (not a struct!) operate by completely different rules, because this is C++ and consistency is for the weak.
{{<pre cpp>}}struct Foo
{
  explicit Foo(int b)
  {
    a = b;
  }
  ~Foo() {}
  
  int a;
};

int get()
{
  static Foo foo(3);
  
  return foo.a;
}

int main()
{
  return get() + get();
}
{{</pre>}}
When is `foo` constructed? It's not when the program starts - it's actually only constructed *the first time the function gets called*. C++ injects some magic code that stores a global flag saying whether or not the static variable has been initialized yet. The first time we call `get()`, it will be false, so the constructor is called and the flag is set to true. The second time, the flag is true, so the constructor isn't called. So when does it get destructed? After `main()` returns and the program is exiting, just like global variables!

Now, this static initialization *is* gauranteed to be thread-safe, but that's only useful if you intend to share the value through multiple threads, which usually doesn't work very well, because only the initialization is thread-safe, not accessing the variable. C++ has introduced a new lifetime called `thread_local` which is even weirder. Thread-local static variables only exist for the duration of the *thread* they belong to. So, if you have a thread-local static variable in a function, it's constructed the first time you call the function *on a per-thread basis*, and destroyed *when each thread exits*, not the program. This means you are gauranteed to have a unique instance of that static variable for each thread, which can be useful in certain concurrency situations.

I'm not going to spend any more time on `thread_local` because to understand it you really need to know how C++ concurrency works, which is out of scope for this blog post. Instead, let's take a brief look at Move Semantics.

### Move Semantics

Let's look at C++'s smart pointer implementation, `unique_ptr<>`. 
{{<pre cpp>}}int get(int* b)
{
  return *b;
}

int main()
{
  std::unique_ptr<int> p(new int());
  *p = 3;
  int a = get(p.get());
  return a;
}
{{</pre>}}
Here, we allocate a new integer on the heap by calling `new`, then store it in `unique_ptr`. This ensures that when our function returns, our integer gets freed and we don't leak memory. However, the lifetime of our pointer is actually excessively long - we don't need our integer pointer after we've extracted the value inside `get()`. What if we could change the lifetime of our pointer? The actual lifetime that we want is this:
{{<pre cpp>}}int get(int* b)
{
  return *b;
  // We want the lifetime to end here
}

int main()
{
  // Lifetime starts here
  std::unique_ptr<int> p(new int());
  *p = 3;
  int a = get(p.get());
  return a;
  // Lifetime ends here
}
{{</pre>}}
We can accomplish this by using **move semantics**:
{{<pre cpp>}}int get(std::unique_ptr<int>&& b)
{
  return *b;
  // Lifetime of our pointer ends here
}

int main()
{
  // Lifetime of our pointer starts here
  std::unique_ptr<int> p(new int());
  *p                = 3;
  int a             = get(std::move(p));
  return a;
  // Lifetime of p ends here, but p is now empty
}
{{</pre>}}
By using `std::move`, we *transfer ownership* of our unique_ptr to the function parameter. Now the `get()` function owns our integer pointer, so as long as we don't move it around again, it will go out of scope once `get()` returns, which will delete it. Our previous `unique_ptr` variable `p` is now empty, and when it goes out of scope, nothing happens, because it gave up ownership of the pointer it contained. This is how you can implement automatic memory management in C++ without needing to use a garbage collector, and Rust actually uses a more sophisticated version of this built into the compiler. 

Move semantics can get very complex and have a lot of rules surrounding how temporary values work, but we're not going to get into all that right now. I also haven't gone into the many different ways that constructors can be invoked, and how those constructors interact with the [different ways you can initialize objects](https://blog.tartanllama.xyz/initialization-is-bonkers/). Hopefully, however, you now have a grasp of what lifetimes are in C++, which is a good jumping off point for learning about more advanced concepts.

---

{{<sup>}}<a name="f1">[1]</a>{{</sup>}} Pedantic assembly-code analysts will remind us that the stack allocations usually happen exactly once, at the beginning of the function, and then are popped off at the very end of the function, but the standard technically doesn't even require a stack to exist in the first place, so we're really talking about pushing and popping off the abstract stack concept that the language uses, not what the actual compiled assembly code really does.

{{<sup>}}<a name="f2">[2]</a>{{</sup>}} We're *dereferencing* the pointer here because we want to return the *value* of the pointer, not the pointer itself! If you tried to return the pointer itself from the function, it would point to freed memory and crash after the function returned. Trying to return pointers from functions is a common mistake, so be careful if you find yourself returning a pointer to something. It's better to use `unique_ptr` to manage lifetimes of pointers for you.