+++
blogimport = true
categories = ["blog"]
comments = [3212571582671363438, 5869540101005377245, 2546076055383486290, 8112796726293723087, 2922828990789017736, 6073057482392914812, 4357224406632415270]
date = "2011-07-21T19:35:00Z"
title = "C# to C++ Tutorial - Part 2: Pointers Everywhere!"
updated = "2013-05-10T02:02:17.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
[ [1](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-1-basics-of-syntax.html) &middot; **2** &middot; [3](http://blackhole12.blogspot.com/2011/09/c-to-c-tutorial-part-3-classes-and.html) &middot; [4](http://blackhole12.blogspot.com/2012/10/c-to-c-tutorial-part-4-operator-overload.html) <span style="color:#aaa;">&middot; 5 &middot; 6 &middot; 7</span> ]

We still have a lot of ground to cover on pointers, but before we do, we need to address certain conceptual frameworks missing from C# that one must be intimately familiar with when moving to C++.

Specifically, in C# you mostly work with the **Heap**. The heap is not difficult to understand - its a giant lump of memory that you take chunks out of to allocate space for your classes. Anything using the {{<code>}}new{{</code>}} keyword is allocated on the heap, which ends up being almost everything in a C# program. However, the heap isn't the only source of memory - there is also the **Stack**. The Stack is best described as what your program lives inside of. I've said before that everything takes up memory, and yes, that includes your program. The thing is that the Heap is inherently dynamic, while the Stack is inherently *fixed*. Both can be re-purposed to do the opposite, but trying to get the Stack to do dynamic allocation is extremely dangerous and is almost guaranteed to open up a mile-wide security hole. 

I'm going to assume that a C# programmer knows what a [stack](http://en.wikipedia.org/wiki/Stack_(data_structure)) is. All you need to understand is that absolutely every single piece of data that isn't allocated on the heap is pushed or popped off your program's stack. That's why most debuggers have a "stack" of functions that you can go up and down. Understanding the stack in terms of how many functions you're inside of is ok, but in reality, there are also variables declared on the stack, including every single parameter passed to a function. It is important that you understand how variable scope works so you can take advantage of declaring things on the stack, and know when your stack variables will simply vanish into nothingness. This is where {{<code>}}{{{</code>}} and {{<code>}}}{{</code>}} come in.

{{<pre cpp>}}int main(int argc, char *argv[])
{
  int bunny = 1;
  
  {
    int carrot=3;
    int lettuce=8;
    bunny = 2; // Legal
  }

  //carrot=2; //Compiler error: carrot does not exist
  int carrot = 3; //Legal, since the other carrot no longer exists
  
  {
    int lettuce = 0;

    { 
       //int carrot = 1; //Compiler error: carrot already defined
       int grass = 9;
       
       bunny = grass; //Still legal
       bunny = carrot; // Also legal
    }
    
    //bunny = grass; //Illegal
    bunny = lettuce; //Legal
  }
  
  //bunny = lettuce; //Illegal
}
{{</pre>}}
{{<code>}}{{{</code>}} and {{<code>}}}{{</code>}} define **scope**. Anything declared inside of them ceases to exist outside, but is still accessible to any additional layers of scope declared inside of them. This is a way to see your program's stack in action. When {{<code>}}bunny{{</code>}} is declared, its pushed on to the stack. Then we enter our first scope area, where we push {{<code>}}carrot{{</code>}} and {{<code>}}lettuce{{</code>}} on to the stack and set {{<code>}}bunny{{</code>}} to 2, which is legal because {{<code>}}bunny{{</code>}} is still on the stack. When the scope is then closed, however, anything declared inside the scope is popped from the stack *in the exact opposite order it was pushed on*. Unfortunately, compiler optimization might change that order behind the scenes, so **don't rely on it**, but it should be fairly consistent in debug builds. First {{<code>}}lettuce{{</code>}} is de-allocated (and its destructor called, if it has one), then {{<code>}}carrot{{</code>}} is de-allocated. Consequently, trying to set {{<code>}}carrot{{</code>}} to 2 outside of the scope will result in a compiler error, because it doesn't exist anymore. This means we can now declare an entirely new integer variable that is also called {{<code>}}carrot{{</code>}}, without causing an error. If we visualize this as a stack, that means {{<code>}}carrot{{</code>}} is now directly above {{<code>}}bunny{{</code>}}. As we enter a new scope area, {{<code>}}lettuce{{</code>}} is then put on top of {{<code>}}carrot{{</code>}}, and then {{<code>}}grass{{</code>}} is put on top of {{<code>}}lettuce{{</code>}}. We can still assign either {{<code>}}lettuce{{</code>}} or {{<code>}}carrot{{</code>}} to {{<code>}}bunny{{</code>}}, since they are all on the stack, but once we leave this inner scope, {{<code>}}grass{{</code>}} is popped off the stack and no longer exists, so any attempt to use it causes an error. {{<code>}}lettuce{{</code>}}, however, is still there, so we can assign {{<code>}}lettuce{{</code>}} to {{<code>}}bunny{{</code>}} before the scope closes, which pops {{<code>}}lettuce{{</code>}} off the stack.

Now the only things on the stack are {{<code>}}bunny{{</code>}} and {{<code>}}carrot{{</code>}}, in that order (if the compiler hasn't moved things around). We are about to leave the function, and the function is also surrounded by {{<code>}}{{{</code>}} and {{<code>}}}{{</code>}}. This is because a function is, itself, a scope, so that means all variables declared inside of that scope are also destroyed in the order they were declared in. First {{<code>}}carrot{{</code>}} is destroyed, then {{<code>}}bunny{{</code>}} is destroyed, and then the function's parameters {{<code>}}argc{{</code>}} and {{<code>}}argv{{</code>}} are destroyed (however the compiler can push those on to the stack in whatever order it wants, so we don't know the order they get popped off), until finally the *function itself* is popped off the stack, which returns program flow to whatever called it. In this case, the function was {{<code>}}main{{</code>}}, so program flow is returned to the parent operating system, which does cleanup and terminates the process.

You can declare anything that has a size determined at compile time on the stack. This means if you have an array that has a constant size, you can declare it on the stack:

{{<pre cpp>}}int array[5]; //Array elements are not initialized and therefore are undefined!
int array[5] = {0,0,0,0,0}; //Elements all initialized to 0
//int array[5] = {0}; // Compiler error - your initialization must match the array size
{{</pre>}}
You can also let the compiler infer the size of the array:

{{<pre cpp>}}int array[] = {1,2,3,4}; //Declares an array of 4 ints on the stack initialized to 1,2,3,4
{{</pre>}}
Not only that, but you can declare class instances and other objects on the stack.

{{<pre cpp>}}Class instance(arg1, arg2); //Calls a constructor with 2 arguments
Class instance; //Used if there are no arguments for the constructor
//Class instance(); //Causes a compiler error! The compiler will think its a function.
{{</pre>}}
<a id="struct-init">In fact</a>, if you have a very simple data structure that uses only default constructors, you can use a shortcut for initializing its members. I haven't gone over classes and structs in C++ yet (See [Part 3](http://blackhole12.blogspot.com/2011/09/c-to-c-tutorial-part-3-classes-and.html)), but here is the syntax anyway:

{{<pre cpp>}}struct Simple
{
  int a;
  int b;
  const char* str;
};

Simple instance = { 4, 5, "Sparkles" };
//instance.a is now 4
//instance.b is now 5
//instance.str is now "Sparkles"
{{</pre>}}
All of these declare variables on the stack. C# actually does this with trivial datatypes like {{<code>}}int{{</code>}} and {{<code>}}double{{</code>}} that don't require a {{<code>}}new{{</code>}} statement to allocate, but otherwise forces you to use the Heap so its garbage collector can do the work.

Wait a minute, stack variables automatically destroy themselves when they go out-of-scope, but how do you delete variables allocated from the Heap? In C#, you didn't need to worry about this because of Garbage Collection, which everyone likes because it reduces memory leaks (but even I have still managed to cause a memory leak in C#). In C++, you must explicitly delete all your variables declared with the {{<code>}}new{{</code>}} keyword, and you must keep in mind which variables were declared as arrays and which ones weren't. In both C# and C++, there are two uses of the {{<code>}}new{{</code>}} keyword - instantiating a single object, and instantiating an array. In C++, there are also two uses of the {{<code>}}delete{{</code>}} keyword - deleting a single object and deleting an array. **You cannot mix up {{<code>}}delete{{</code>}} statements!**

{{<pre cpp>}}int* Fluffershy = new int();
int* ponies = new int[10];

delete Fluffershy; // Correct
//delete ponies; // WRONG, we should be using delete [] for ponies
delete [] ponies; // Just like this
//delete [] Fluffershy; // WRONG, we can't use delete [] on Fluffershy because we didn't
                        // allocate it as an array.

int* one = new int[1];

//delete one; // WRONG, just because an array only has one element doesn't mean you can
              // use the normal delete!
delete [] one; // You still must use delete [] because you used new [] to allocate it.
{{</pre>}}
As you can see, it is much easier to deal with stack allocations, because they are automatically deallocated, even when the function terminates unexpectedly. {{<code>}}[std::auto_ptr](http://www.cplusplus.com/reference/std/memory/auto_ptr/){{</code>}} takes advantage of this by taking ownership of a pointer and automatically deleting it when it is destroyed, so you can allocate the {{<code>}}auto_ptr{{</code>}} on the stack and benefit from the automatic destruction. However, in {{<code>}}C++0x{{</code>}}, this has been superseded by {{<code>}}[std::unique_ptr](http://msdn.microsoft.com/en-us/library/ee410601.aspx){{</code>}}, which operates in a similar manner but uses some complex move semantics introduced in the new standard. I won't go into detail about how to use these here as its out of the scope of this tutorial. Har har har.

For those of you who like throwing exceptions, I should point out common causes of memory leaks. The most common is obviously just flat out forgetting to delete something, which is usually easily fixed. However, consider the following scenario:

{{<pre cpp>}}void Kenny()
{
  int* kenny = new int();
  throw "BLARG";
  delete kenny; // Even if the above exception is caught, this line of code is never reached.
}

int main(int argc, char* argv[])
{
  try {
  Kenny();
  } catch(char * str) { 
    //Gotta catch'em all.
  }
  return 0; //We're leaking Kenny! o.O
}
{{</pre>}}
Even this is fairly common:

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* kitty = new int();

  *kitty=rand();
  if(*kitty==0)
    return 0; //LEAK
  
  delete kitty;
  return 0;
}
{{</pre>}}
These situations *seem* obvious, but they will happen to you once the code becomes enormous. This is one reason you have to be careful when inside functions that are very large, because losing track of {{<code>}}if{{</code>}} statements may result in you forgetting what to delete. A good rule of thumb is to make sure you delete everything whenever you have a return statement. However, the opposite can also happen. If you are too vigilant about deleting everything, you might delete something you never allocated, which is just as bad:

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* rarity = new int();
  int* spike;

  if(rarity==NULL)
  {
    spike=new int();
  }
  else
  {
    delete rarity;
    delete spike; // Suddenly, in an alternate dimension, earth ceased to exist
    return 0;
  }
  
  delete rarity; // Since this only happens if the allocation failed and returned a NULL
                 // pointer, this will also blow up.
  delete spike;
  return 0;
}
{{</pre>}}
Clearly, one must be careful when dealing with allocating and destroying memory in C++. Its usually best to encapsulate as much as possible in classes that automate such things. But wait, what about that {{<code>}}NULL{{</code>}} pointer up there? Now that we're familiar with memory management, we're going to dig into pointers again, starting with the {{<code>}}NULL{{</code>}} pointer.

Since a pointer points to a piece of memory that's somewhere between 0 and 4294967295, what happens if its pointing at 0? Any pointer to memory location 0 **is always invalid**. All you need to know is that the operating system does some magic voodoo to ensure that any attempted access of memory location 0 will always throw an error, no matter what. 1, 2, 3, and any other double or single digit low numbers are also always invalid. {{<code>}}0xfdfdfdfd{{</code>}} is what the VC++ debugger sets uninitialized memory to, so that pointer location is also always invalid. A pointer set to 0 is called a **Null Pointer**, and is usually used to signify that a pointer is empty. Consequently if an allocation function fails, it tends to return a null pointer. Null pointers are returned when the operation failed and a valid pointer cannot be returned. Consequently, you may see this:

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* blink = new int();
  if(blink!=0) delete blink;
  blink=0;
  return 0;
}
{{</pre>}}
This is known as a **safe deletion**. It ensures that you only delete a pointer if it is valid, and once you delete the pointer you set the pointer to 0 to signify that it is invalid. Note that {{<code>}}NULL{{</code>}} is defined as 0 in the standard library, so you could also say {{<code>}}blink = NULL{{</code>}}.

Since pointers are just integers, we can do **pointer arithmetic**. What happens if you add 1 to a pointer? If you think of pointers as just integers, one would assume it would simply move the pointer forward a single byte.

{{<img src="/img/tut2.png" alt="Moving a Pointer 1 byte" >}}

This isn't what happens. Adding 1 to a pointer of type {{<code>}}integer{{</code>}} results in the pointer moving forward 4 bytes.

{{<img src="/img/tut3.png" alt="Moving a Pointer 4 bytes" >}}

**Adding or subtracting an integer $i$ from a pointer moves that pointer $i\cdot n$ bytes, where $n$ is the size, in bytes, of the pointer's type**. This results in an interesting parallel - adding or subtracting from a pointer is the same as treating the pointer as an array and accessing it via an index.

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* kitties = new int[14];
  int* a = &kitties[7];
  int* b = kitties+7; //b is now the same as a
  int* c = &a[4];
  int* d = b+4; //d is now the same as c
  int* e = &kitties[11];
  int* f = kitties+11; 
  //c,d,e, and f now all point to the same location
}
{{</pre>}}
So pointer arithmetic is identical to accessing a given index and taking the address. But what happens when you try to add two pointers together? Adding two pointers together is undefined because it tends to produce total nonsense. *Subtracting* two pointers, however, is defined, provided you subtract a smaller pointer from a larger one. The reason this is allowed is so you can do this:

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* eggplants = new int[14];
  int* a = &eggplants[7];
  int* b = eggplants+10;
  int diff = b-a; // Diff is now equal to 3
  a += (diff*2); // adds 6 to a, making it point to eggplants[13]
  diff = a-b; // diff is again equal to 3
  diff = a-eggplants; //diff is now 13
  ++a; //The increment operator is valid on pointers, and operates the same way a += 1 would
  // So now a points to eggplants[14], which is not a valid location, but this is still
  // where the "end" of the array technically is.
  diff = a-eggplants; // Diff now equals 14, the size of the array
  --b; // Decrement works too
  diff = a-b; // a is pointing to index 14, b is pointing to 9, so 14-9 = 5. Diff is now 5.
  return 0;
}
{{</pre>}}
There is a mistake in the code above, can you spot it? I used a *signed* {{<code>}}integer{{</code>}} to store the difference between the two pointers. What if one pointer was above 2147483647 and the other was at 0? The difference would overflow! Had I used an unsigned integer to store the difference, I'd have to be really damn sure that the left pointer was larger than the right pointer, or the negative value would *also* overflow. This complexity is why you have to goad windows into letting your program deal with pointer sizes over 2147483647.

In addition to arithmetic, one can compare two pointers. We already know we can use {{<code>}}=={{</code>}} and {{<code>}}!={{</code>}}, but we can also use {{<code>}}< > <={{</code>}} and {{<code>}}>={{</code>}}. While you can get away with comparing two completely unrelated pointers, these comparison operators are usually used in a context like the following:

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* teapots = new int[15];
  int* end = teapots+15;
  for(int* s = teapots; s<end; ++s)
    *s = 0;
  return 0;
}
{{</pre>}}
Here the for loop increments the pointer itself rather than an index, until the pointer reaches the end, at which point it terminates. But, what if you had a pointer that didn't have any type at all? {{<code>}}void*{{</code>}} is a legal pointer type, that any pointer type can be implicitly converted to. You can also explicitly cast {{<code>}}void*{{</code>}} to any pointer type you want, which is why you are allowed to explicitly cast any pointer type to another pointer type ({{<code>}}int* p; short* q = (short*)p;{{</code>}} is entirely legal). Doing so, however, is obviously dangerous. {{<code>}}void*{{</code>}} has its own problems, namely, how big is it? The answer is, you don't know. **Any attempt to use any kind of pointer arithmetic with a {{<code>}}void*{{</code>}} pointer will cause a compiler error**. It is most often used when copying generic chunks of memory that only care about size in bytes, and not what is actually contained in the memory, like {{<code>}}memcpy(){{</code>}}.

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int* teapots = new int[15];
  void* p = (void*)teapots;
  p++; // compiler error
  unsigned short* d = (unsigned short*)p;
  d++; // No compiler error, but you end up pointing to half an integer
  d = (unsigned short*)teapots; // Still valid
  return 0;
}
{{</pre>}}
Now that we know all about pointer manipulation, we need to look at pointers to pointers, and to anchor this in a context that actually makes sense, we need to look at how C++ does **multidimensional arrays**. In C#, [multidimensional arrays](http://msdn.microsoft.com/en-us/library/2yd9wwz4(v=vs.71).aspx) look like this:

{{<pre csharp>}}int[,] table = new int[4,5];
{{</pre>}}
C++ has a different, but fairly reasonable stack-based syntax. When you want to declare a multidimensional array *on the heap*, however, things start getting *weird*:

{{<pre cpp>}}int unicorns[5][3]; // Well this seems perfectly reasonable, I wonder what-
  int (*cthulu)[50] = new int[10][50]; // OH GOD GET IT AWAY GET IT AWAAAAAY...!
  int c=5;
  int (*cthulu)[50] = new int[c][50]; // legal
  //int (*cthulu)[] = new int[10][c]; // Not legal. Only the leftmost parameter
                                      // can be variable
  //int (*cthulu)[] = new int[10][50]; // This is also illegal, the compiler is not allowed
                                       // to infer the constant length of the array.
{{</pre>}}
Why isn't the multidimensional array here just an {{<code>}}int**{{</code>}}? Clearly if {{<code>}}int* x{{</code>}} is equivalent to {{<code>}}int x[]{{</code>}}, shouldn't {{<code>}}int** x{{</code>}} be equivalent to {{<code>}}int x[][]{{</code>}}? Well, it *is* - just look at the {{<code>}}main(){{</code>}} function, its got a multidimensional array in there that can be declared as just {{<code>}}char** argv{{</code>}}. The problem is that there are two kinds of multidimensional arrays - **square** and **jagged**. While both are accessed in identical ways, how they work is fundamentally different.

Let's look at how one would go about allocating a 3x5 square array. We can't allocate a 3x5 chunk out of our computer's memory, because memory isn't 2-dimensional, its 1-dimensional. Its just freaking huge line of bytes. Here is how you squeeze a 2-dimensional array into a 1-dimensional line:

{{<img src="/img/tut4.png" alt="Allocating a 3x5 array" >}}

As you can see, we just allocate each row right after the other to create a 15-element array ($5\cdot 3 = 15$). But then, how do we access it? Well, if it has a width of 5, to access another "row" we'd just skip forward by 5. In general, if we have an $n$ by $m$ multidimensional array being represented as a one-dimensional array, the proper index for a coordinate $(x,y)$ is given by: {{<code>}}array[x + (y*n)]{{</code>}}. This can be extended to 3D and beyond but it gets a little messy. This is all the compiler is really doing with multidimensional array syntax - just automating this for you.

Now, if this is a **square** array (as evidenced by it being a square in 2D or a cube in 3D), a jagged array is one where each array is a different size, resulting in a "jagged" appearance:

{{<img src="/img/tut5.png" alt="Jagged array visualization" >}}

We can't possibly allocate this in a single block of memory unless we did a lot of crazy ridiculous stuff that is totally unnecessary.  However, given that arrays in C++ are just pointers to a block of memory, what if you had a pointer to a block of memory that was an array of pointers to more blocks of memory?

{{<img src="/img/tut6.png" alt="Jagged array pointers" >}}

Suddenly we have our jagged array that can be accessed just like our previous arrays. It should be pointed out that with this format, each inner-array can be in a totally random chunk of memory, so the last element could be at position 200 and the first at position 5 billion. Consequently, pointer arithmetic only makes sense within each column. Because this is an array of arrays, we declare it by creating an array of pointers. This, however, does **not** initialize the entire array; all we have now is an array of *illegal pointers*. Since each array could be a different size than the other arrays (this being the entire point of having a jagged array in the first place), the only possible way of initializing these arrays is individually, often by using a {{<code>}}for loop{{</code>}}. Luckily, the syntax for accessing jagged arrays is the exact same as with square arrays.

{{<pre cpp>}}int main(int argc, char* argv[])
{
  int** jagged = new int*[5]; //Creates an array of 5 pointers to integers.
  for(int i = 0; i < 5; ++i)
  {
    jagged[i] = new int[3+i]; //Assigns each pointer to a new array of a unique size
  }
  jagged[4][1]=0; //Now we can assign values directly, or...
  int* second = jagged[2]; //Pull out one column, and
  second[0]=0; //manipulate it as a single array

  // The double-access works because of the order of operations. Since [] is just an
  // operator, it is evaluated from left to right, like any other operator. Here it is
  // again, but with the respective types that each operator resolves to in parenthesis.
  ( (int&) ( (int*&) jagged[4] ) [1] ) = 0;
}
{{</pre>}}As you can see above, just like we can have pointers to pointers, we can also have references to pointers, since pointers are just another data type. This allows you to re-assign pointer values inside jagged arrays, like so: {{<code>}}jagged[2] = (int*)kitty{{</code>}}. However, until {{<code>}}C++0x{{</code>}}, those references didn't have any meaningful data type, so even though the compiler was using {{<code>}}int*&{{</code>}}, using that in your code will throw a compiler error in older compilers. If you need to make your code work in non-{{<code>}}C++0x{{</code>}} compilers, you can simply avoid using references to pointers and instead use a pointer to a pointer.  {{<pre cpp>}}int* bunny;
int* value = new int[5];

int*& bunnyref = bunny; // Throws an error in old compilers
int** pbunny = &bunny; // Will always work
bunnyref = value; // This does the same exact thing as below.
*pbunny = value;

// bunny is now equal to value
{{</pre>}}This also demonstrates the other use of a pointer-to-pointer data type, allowing you to remotely manipulate a pointer just like a pointer allows you to remotely manipulate an integer or other value type. So obviously you can do pointers to pointers to pointers to pointers to an absurd degree of lunacy, but this is *exceedingly rare* so you shouldn't need to worry about it.  Now you should be strong in the art of pointer-fu, so our next tutorial will finally get into object-oriented techniques in C++ in comparison to C#.   [Part 3: Classes and Structs and Inheritance OH MY!](http://blackhole12.blogspot.com/2011/09/c-to-c-tutorial-part-3-classes-and.html)
