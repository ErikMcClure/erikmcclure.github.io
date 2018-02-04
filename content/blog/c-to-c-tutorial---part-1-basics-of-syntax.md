+++
title = "C# to C++ Tutorial - Part 1: Basics of Syntax"
date = 2011-07-06T04:26:00Z
updated = 2013-05-10T02:02:50Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

[ **1** &middot; [2](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-2-pointers.html) &middot; [3](http://blackhole12.blogspot.com/2011/09/c-to-c-tutorial-part-3-classes-and.html) &middot; [4](http://blackhole12.blogspot.com/2012/10/c-to-c-tutorial-part-4-operator-overload.html) <span style="color:#aaa;">&middot; 5 &middot; 6 &middot; 7</span> ]

When moving from C# to C++, one must have a very deep knowledge of what C# is actually doing when you run your program. Doing so allows you to recognize the close parallels between both languages, and why and how they are different. This tutorial will assume you have a fairly strong grasp of C#, but may not be familiar with some of its more arcane attributes.

In C#, everything is an object, or a static member of an object. You can't have a function just floating around willy-nilly. However, like all programs, a C# program must have an entry-point. If you have primarily done GUI-based design, you probably aren't aware of the entry-point that is automatically generated, but it is definitely there, and like everything else, it's part of an object. C# actually allows you to change the entry point function, but a default C# project will automatically generate a Program.cs file that looks like this:

{{<pre csharp>}}using System;
using System.Collections.Generic;
using System.Linq;
using System.Windows.Forms;

namespace ScheduleTimer
{
  static class Program
  {
    /// <summary>
    /// The main entry point for the application.
    /// </summary>
    [STAThread]
    static void Main()
    {
      Application.EnableVisualStyles();
      Application.SetCompatibleTextRenderingDefault(true);
      Application.Run(new frmMain());
    }
  }
}
{{</pre>}}
{{<code>}}static void Main(){{</code>}} is the real entry point for your application, which simply initializes visual styles and then immediately launches the form that most C# users are accustomed to working with. Now, we can compare this with a simple "Hello World" C++ program:

{{<pre cpp>}}#include <iostream>

int main(int argc, char *argv[])
{
  std::cout << "Hello World";
  return 0;
}
{{</pre>}}This program, to a C# user, immediately looks foreign and possibly even outright hostile. However, almost everything in it has a direct analogue in C#, despite the rather inane syntax that is being used. The most glaring example here is the insertion operator, <<, because almost no one ever uses it except for in streams and the fact that it's in a C++ Hello World program creates an absurd amount of confusion. It's just a fancy way of doing this:  {{<pre cpp>}}
#include <iostream>

int main(int argc, char *argv[])
{
  std::cout.write("Hello World",11);
  return 0;
}
{{</pre>}}
Now, counting the number of bytes you are pumping into the stream is really annoying, and that's what the insertion operator does for you; it properly formats everything automatically. That's all. It's not a demon from hell bent on destroying your life, its just weird syntax. I don't know why they don't also have this functionality in a much easier to understand overloaded function, but there are a lot of things that they don't do, so we'll just have to live with it.

The {{<code>}}main(){{</code>}} function here serves the same exact purpose as the {{<code>}}Main(){{</code>}} function in C#. Strict C++ requires you to have a {{<code>}}main(){{</code>}} function to serve as an entry point, but various operating systems modify it and, in the case of Windows, outright replace it. As such, you will notice that your "hello world" C++ program, when built, opens in a command line. You will learn later how to prevent this by using Windows' proprietary entry function. For those of you familiar with C#, this is exactly the same as C#'s ability to change around the entry point of the application, and you can even make a command line application in C# too by properly changing the compiler settings. The same concept applies to C++, but unlike C#, which defaults to a GUI, C++ defaults to a command line. Changing the compiler settings properly will result in a C++ program that starts in a GUI, just like C# (although unlike C#, C++ doesn't have any help, which turns GUI programming into a complete nightmare).

So now that we have a direct analogue between C# and C++ in terms of where our application *starts*, we need to deal with a conceptual difference in how C# and C++ handle *dependencies*. In C#, your class file is just {{<code>}}Class.cs{{</code>}}, your helper class is {{<code>}}Helper.cs{{</code>}}, and both of them can call the other one provided they are in the same namespace, or if you are inheriting someone else's, using the correct {{<code>}}using{{</code>}} statements to resolve the code. If these concepts are not familiar to you, you should learn more about C# before delving further into C++.

C++, on the other hand, does not do behind-the-scenes magic to help you resolve your dependencies. To understand what C++ is doing, one must understand how any compiler resolves references inside code (including C#). When the C# compiler is compiling your project, it goes through each of your code files **one by one** and compiles everything to an intermediate object code that will later be compiled down into the machine code (or, in this case, bytecode, since C# is an interpreted language). But wait, what if it's compiling {{<code>}}Class.cs{{</code>}} before {{<code>}}Helper.cs{{</code>}} even though {{<code>}}Class{{</code>}} instantiates a {{<code>}}Helper{{</code>}} object and calls some functions inside of it that then instantiate another {{<code>}}Class{{</code>}} object? Well, what if you compiled {{<code>}}Helper.cs{{</code>}} first... but {{<code>}}Helper.cs{{</code>}} needs {{<code>}}Class.cs{{</code>}} to be compiled first because its instantiating a {{<code>}}Class{{</code>}} object inside the function that the {{<code>}}Class{{</code>}} object is calling! That's a circular dependency! *<del>THIS IS IMPOSSIBLE OH GOD WE'RE GOING TO DIE</del>* No, it's actually quite simple to deal with. Enter *prototypes*. If you have the following C# class: 

{{<pre csharp>}}using System;

namespace FunFunBunBuns
{
  class Class
  {
    private int _yay;
    private int _bunnies;

    // Constructor
    public Class(int yay)
    {
      _yay = yay;
      _bunnies = 0; // :C
    }

    // Destructor
    public ~Class()
    {
      _yay = 0;
    }

    public void IncrementYay()
    {
      _yay++;
    }
    
    public int MakeBunnies(int num) // :D
    {
      _bunnies = _bunnies + num;
      return _bunnies;
    }
  }
}
{{</pre>}}
Making "prototypes" of these functions (which C# doesn't have so this will be invalid syntax) would be the following:

{{<pre csharp>}}using System;

namespace FunFunBunBuns
{
  class Class
  {
    private int _yay;
    private int _bunnies;

    // Constructor
    public Class(int yay);
    // Destructor
    public ~Class();
    public void IncrementYay();
    public int MakeBunnies(int num);
  }
}
{{</pre>}}
Notice the distinct lack of code - this is how circular references get resolved. It turns out that to properly compile your program, the compiler only has to know what a function takes in as arguments, and what it returns. By treating the function as a "black box" of sorts, the compiler can ignore whatever code is inside it. Notice that this applies to constructors and destructors as well - they are simply special functions inside the class. In this manner the entire class can be treated as a bunch of black-box functions that don't actually have any code that needs to be compiled in them. What the C# compiler does is create a bunch of these prototypes behind the scenes and feed them in front of all your code files, so it first compiles {{<code>}}Class.cs{{</code>}} using a prototype of the {{<code>}}Helper{{</code>}} class, which allows it to instantiate and use any functions that {{<code>}}Helper{{</code>}} defines without actually knowing the code inside them. Then, it compiles {{<code>}}Helper.cs{{</code>}}, compiling assigning code to the previously empty black-box functions defined in the {{<code>}}Helper{{</code>}} prototype, using a prototype of {{<code>}}Class{{</code>}} so that it can also instantiate and call functions from {{<code>}}Class{{</code>}}. In this way, both {{<code>}}Helper.cs{{</code>}} and {{<code>}}Class.cs{{</code>}} can be compiled in any order.

But wait, what if {{<code>}}Class{{</code>}} inherits {{<code>}}Helper{{</code>}}? In reality, this changes nothing. An important lesson here is that, in C++, you will not be able to simply ignore the fact that *everything is a function*. Classes are just an abstraction - in reality, inheritance, constructors, deconstructors, operators, everything is just various special functions. Python's class syntax is interesting because requires that all class functions explicitly define the {{<code>}}self{{</code>}} parameter (which is identical to the {{<code>}}this{{</code>}} reference in C++ and C#), even the class constructor. Both C++ and C# hide all this from you, so Constructors and Destructors and class functions all magically just work, even though underneath it all they're just ordinary functions with a special parameter that's hidden from view. This is, in fact, how one mimics class behavior in C, which does not have object-oriented features - simply build a struct and make a bunch of functions for it that take a "this" pointer, or a pointer to a specific struct on which the function operates. This behavior can be (needlessly) duplicated using C# - let's transform our {{<code>}}Class{{</code>}} class to C-style function implementations, ignoring the slightly invalid C# syntax.

{{<pre csharp>}}using System;

namespace FunFunBunBuns
{
  struct Class
  {
    private int _yay;
    private int _bunnies;
  };

  public Constructor(Class this, int yay)
  {
    this._yay = yay;
    this._bunnies = 0; // :C
  }

  public Destructor(Class this)
  {
    this._yay = 0;
  }

  public void IncrementYay(Class this)
  {
    this._yay++;
  }
    
  public int MakeBunnies(Class this, int num) // :D
  {
    this._bunnies = this._bunnies + num;
    return this._bunnies;
  }
}
{{</pre>}}
Thankfully, we don't have to worry about this, since thinking of class functions as functions that operate on the object is a lot more intuitive. However, one must be aware that even in inheritance scenarios, everything is just a function, or an overload of a virtual function, or something similar (if you do not know what virtual functions are, you need to learn more C# before proceeding). Consequently, our ability to declare function prototypes solves *all* the dependency issues, because *everything* is a function.

This is where we get into exactly what the {{<code>}}#include{{</code>}} directive is for. In C#, all your files are automatically accessible from every other file, and this isn't a problem because compilation is nigh-instantaneous. C++ is much more intensive to compile, partially because it doesn't have a precompiled 400 MB library of crap to work off of, and partially due to a much more complicated precompiler. That means in C++, if you want a given file to have access to another file, you have to {{<code>}}#include{{</code>}} that file. In our Hello World application, we are including {{<code>}}iostream{{</code>}}, which does not have a .h file extension on the end for stupid regulatory reasons. However, what about the file our code is in? Our code is not in a .h file, its in a .cpp file. This is where we get to a critical difference between C# and C++. While C# just has .cs files for code, **C++ has two types of files: header files and code files**.

{{<code>}}.cpp == C++ (C-plus-plus) code file
.h = C++ Header file{{</code>}}

Header files contain class and function prototypes, and code files contain all the actual code. A C++ project is therefore defined entirely by a list of .cpp files that need to be compiled. Header files are just little helper files that make resolving dependencies easier. C# does this for you - C++ does not. Note that because these are technically arbitrary file distinctions, you can put whatever you want in either file type; nothing will stop you from doing {{<code>}}#include "main.cpp"{{</code>}}, its just ridiculous and confusing. Both {{<code>}}#include <>{{</code>}} and {{<code>}}#include ""{{</code>}} are valid syntax for the {{<code>}}#include{{</code>}} directive, there is no *real* difference. Standard procedure, however, is that {{<code>}}#include <>{{</code>}} is used for any header files outside of your project, and {{<code>}}#include ""{{</code>}} is used for header files inside your project, or closely related to it.

So what we're doing when we say {{<code>}}#include <iostream>{{</code>}} is that we're including a bunch of prototypes for various input/output stream (i/o stream --> iostream) related classes defined in the standard library, which your compiler already has the corresponding .cpp implementations of built into it. So, the compiler links the application against this header file, and when you use {{<code>}}std::cout{{</code>}}, it just treats everything in it (including that ridiculously obtuse {{<code>}}<< operator{{</code>}}, which is really just another function) as a black-box function.

<a id="header-code">Consequently</a>, unless you know what your doing, you should keep code out of header files. C++ doesn't prevent you from throwing functions that aren't attached to classes all over the place, like C# does, so what would happen if you defined {{<code>}}int ponies() { return 0; }{{</code>}} in a header file that you include in two seperate .cpp files? The compiler will try to compile the function twice, and on the second time it will explode because the function it tried to put code into already had code in it, since it wasn't a prototype! **EVERYTHING DIES!** So until you get to the more advanced areas of C++, don't put code in your header files (unless you want to watch your compiler die, you monster).

At this point I want to clarify what {{<code>}}std::{{</code>}} is, because it looks rather weird to a C# programmer. In C#, the {{<code>}}. operator{{</code>}} works on everything - you just have {{<code>}}System.Forms.Whatever.Help.Im.Trapped.In.A.Universe.Factory.Your.Class.Member.Function(){{</code>}} and its all good. In C++, that's not going to work anymore. The {{<code>}}:: operator{{</code>}} is known as the Scope Resolution Operator. It's a lot easier to explain if I first explain what the {{<code>}}. operator{{</code>}} has been demoted to. You can only use the {{<code>}}. operator{{</code>}} on a *reference or value type* of an *instantiated object* (basically everything you've ever worked with in C#). The important distinction here is that **static functions cannot be accessed with the {{<code>}}. operator{{</code>}} anymore**. This is because Static functions, along with namespaces and typedefs and *everything else* must use the Scope Resolution Operator. Consequently, you can think of the {{<code>}}. operator{{</code>}} as being demoted to just calling class functions, and everything else now uses the {{<code>}}:: operator{{</code>}}. So, {{<code>}}std::cout{{</code>}} just means that we're access the {{<code>}}cout{{</code>}} class in the {{<code>}}std{{</code>}} namespace.

Now we just have one more hurdle to overcome with the "Hello World" application, that funky {{<code>}}char* argv[]{{</code>}} parameter in {{<code>}}main(){{</code>}}. Most C# programmers can correctly infer that it is probably an array of some sort, but we don't know what type {{<code>}}char*{{</code>}} is, other than its clearly related to {{<code>}}char{{</code>}}.

{{<code>}}char*{{</code>}} is a **pointer**. Yes, the same scary pointers you hear about all the time. No, they aren't really scary. In fact, you have been using similar concepts in C# all the time without actually realizing it. First, however, let's take a hard look at what a pointer really is.

Everything in your entire program takes up **memory**. Since this tutorial is designed for people who know C# already, I really, really hope you already knew that. What you might not know is that all this memory has a specific location on the machine. In fact, on a 32-bit machine, **every single possible location of a byte** can be contained in **an unsigned 32-bit integer**. This is why we are currently moving to 64-bit CPUs, because an unsigned 32-bit integer can only hold up to 4294967295 possible byte locations, which amounts to 4.2 gigs of memory. That's why you are limited to 4 gigs of RAM on a 32-bit machine, and windows has difficulty using more than 2 gigs because a lot of older programs assumed that a **signed** 32-bit integer was sufficient for all memory addresses, so windows has to do some funky memory paging techniques to get programs that ignore the last bit to use memory locations above 2147483647.

So, if you allocate a float, either on the stack or on the heap, *it must exist somewhere within those 4294967295 possible byte locations*. Consequently, lets say you want to call a function that modifies that float, but the function has to have a void return value for some arbitrary reason. If you know where in memory that float is, you can tell the function where to find the float and modify it to the desired value without ever returning a value. Here is an example C++ function doing just that (which is syntactically valid all by itself because C++ allows functions outside of classes):

{{<pre cpp>}}void ModifyFloat(float* p)
{
  *p = 100.0;
}

int main(int argc, char* argv[])
{
  float x = 0; //x is equal to 0.0
  ModifyFloat( &x );
  // x is now equal to 100.0
}
{{</pre>}}
What's going on here? First, we have our {{<code>}}ModifyFloat(){{</code>}} function. This takes a pointer to a {{<code>}}float{{</code>}}, which is declared by adding a {{<code>}}*{{</code>}} to the desired type we want to make a pointer to. Remember that pointers are really just 32-bit integers (or 64-bit if you have a 64-bit operating system), but C++ assigns them a type so that if you try to assign a {{<code>}}double{{</code>}} to a pointer to a {{<code>}}float{{</code>}}, it throws an error instead of overflowing 4 extra bytes, causing a heap corruption and destroying the universe. So {{<code>}}char*{{</code>}} is a pointer to a {{<code>}}char{{</code>}}, a {{<code>}}double*{{</code>}} points to a {{<code>}}double{{</code>}}, and {{<code>}}Helper*{{</code>}} is a pointer to our own {{<code>}}Helper{{</code>}} class.

The next thing done in {{<code>}}ModifyFloat(){{</code>}} is {{<code>}}*p{{</code>}}. In this case, the {{<code>}}* operator{{</code>}} is the *dereference* operator. So unfortunately {{<code>}}*{{</code>}} is the multiply, pointer, and dereference operator in C++. Yes, this is retarded. I'm sorry. But what the heck does dereference even mean? It takes a pointer type and turns it into a *reference*. You already know what a reference is, even if you don't realize it. In C#, you can pass a variable of your {{<code>}}Helper{{</code>}} class into a function, modify the class in the function, and the original variable will get modified too! This is because, by default, classes are passed *by-reference* in C#. That means, even though it looks identical to a variable passed *by value*, any changes made to the variable are in fact made to whatever variable it references. So, this idea of passing variables in by reference should be familiar to an experienced C# programmer. C++ has references too, I just haven't gone over their syntax. Here's a more explicit version of the function:

{{<pre cpp>}}void ModifyFloat(float* p)
{
  float& ref_p = *p;
  ref_p = 100.0;
}
{{</pre>}}
This is the exact same as the previous function, but here we can clearly see the reference. In C#, if you wanted a variable normally passed by value, like a struct, to get passed by reference, you had to override the default behavior by adding {{<code>}}ref{{</code>}}. In C++, a variable that is a reference to a given type is declared in a similar manner to a pointer. The {{<code>}}& operator{{</code>}} is used instead of {{<code>}}*{{</code>}}, so in this example, float& is a reference to a {{<code>}}float{{</code>}}. We assign it to the value produced by turning our pointer into a {{<code>}}float{{</code>}} reference. Then we just set our reference equal to 100.0 and it magically alters the original variable, just like it would in C#. In fact, here is the same function written in (slightly illegal) C#:

{{<pre csharp>}}public static void ModifyFloat(ref float p)
{
  p=100.0;
}
{{</pre>}}
This does the same thing, just without the pointer. In fact, we can totally ignore the pointer in C++ too, if we want (which I tend to prefer, when possible, because its a lot easier to work with):

{{<pre cpp>}}void ModifyFloat(float& ref_p)
{
  ref_p = 100.0;
}
int main(int argc, char* argv[])
{
  float x = 0; //x is equal to 0.0
  ModifyFloat( x );
  // x is now equal to 100.0
}
{{</pre>}}
Now, in this implementation, you will notice that our call to {{<code>}}ModifyFloat{{</code>}} is now equivalent to what it would be in C#, in that we just pass in the variable. What happened to that random {{<code>}}& operator{{</code>}} we had there before? The {{<code>}}& operator{{</code>}} is also known as the {{<code>}}address-of operator{{</code>}}, meaning when its applied to a variable as opposed to a type, it returns a pointer to that variable (yay, more context-dependent redundant operators). So, we could rewrite our function as follows to make it a bit more clear:

{{<pre cpp>}}void ModifyFloat(float* p)
{
  float& ref_p = *p; //get a reference from the pointer
  ref_p = 100.0; //modify the reference
}
int main(int argc, char* argv[])
{
  float x = 0; //x is equal to 0.0
  float* p_x = &x; //get a pointer to x
  ModifyFloat( p_x ); //pass pointer into function
  // x is now equal to 100.0 
}
{{</pre>}}
As we can see, pointers are just the underlying work behind references. If you ever go into Managed C++, you'll find out that all C# references are really just pointers, but the language treats them as references so they're hidden from you. In C++, you can have both pointers and references. It is important to note that you can only *initialize* a reference variable. Any subsequent operators will be applied to whatever variable its referencing, making it impossible to get the address of a reference variable or do anything to the reference variable itself - for all intents and purposes, it just *is* the variable it references. This is why pointers are handy - you CAN reassign the actual pointer variable while also accessing the variable its pointing to. Consequently, you can also get the address of a pointer variable, since just like any other variable, including the reference variable, it must occupy memory, and therefore has a location that you can get a pointer to (we'll get to that syntax in a minute). But there's one more thing...

**What about arrays?** In C#, arrays are actually a built-in class that has lots of fancy functions and whatnot. Interestingly, they are still of fixed size. C++ arrays are also fixed size, but they are manipulated as raw memory. Let's compare initializing an array in C++ and initializing an array in C#:

{{<pre csharp>}}int[] numbers = new int[5];
{{</pre>}}{{<pre cpp>}}int* numbers = new int[5];
{{</pre>}}
It should be pretty obvious at this point that *arrays are pointers* in C++. I can even rewrite the above in C++ using an empty array syntax, and it will be equally as valid:

{{<pre cpp>}}int numbers[] = new int[5];
{{</pre>}}
**{{<code>}}int *x*[]{{</code>}} is identical to {{<code>}}int* *x*{{</code>}}**. There is no difference. Observe the following modification of our original Hello World function:

{{<pre cpp>}}int main(int argc, char** argv);
{{</pre>}}
Same thing. In fact, if you watch your compiler output carefully, you might even see the compiler internally convert all the arrays to pointers when its resolving types. Now, as a C# programmer, you should already know what arrays are. You should probably also be at least dimly aware that each element of an array occupies memory directly after the element proceeding it. So, if you know where the address of the *first* element is, you know the second element is exactly *x* bytes afterwards, where *x* is the number of bytes your type takes up. This is why pointers have types associated with them - we know that {{<code>}}float*{{</code>}} points to an array of elements, and that each element takes up 4 bytes. To verify this, the {{<code>}}sizeof(){{</code>}} built-in function/operator/whatever will return the number of bytes a given type, class, or struct takes up. That's the number of bytes we skip ahead to get to the next element in an array. This is all done transparently in C++ using the same array index operator as C# uses:

{{<pre cpp>}}int main(int argc, char** argv)
{
  int* ponies = new int[5];
  ponies[0] = 1; //First element..
  ponies[1] = 2; //Second element...
}
{{</pre>}}
So pointers can be treated as arrays that behave exactly the same way a C# array does. However, the astute C# programmer would ask, how do you know how long the array is?

{{<span style="color:#f00">}}<h5>YOU DON'T</h5>
{{</span>}}
Enter every single buffer overflow error that has been the bane of man since the beginning of time. *YOU* have to keep track of how long the array is, and you'd better be damn sure you don't get it wrong. Consequently any function taking an array of variable size will also require a separate argument telling the function how many elements are in the array. Usually arrays are just constructed on the stack with a constant, known size, which is often harmless and pretty hard to screw up. If you start doing funky things with them, though, you might want to look up [std::vector](http://www.cplusplus.com/reference/stl/vector/) for an encapsulated dynamic array.

So C++ arrays are just like C# arrays, except they are pointers to the first element, and you don't know how long they are (and they might cause the destruction of the universe if you screw up). You should already know that a string is an array, and consequently in C++ the standard string type is {{<code>}}const char*{{</code>}}, not {{<code>}}string{{</code>}}. You also can't put them in switch() statements. Sorry.

There's a lot of stuff about pointers that this tutorial hasn't covered, like function pointers and pointer arithmetic, which we'll get to next time.

[Part 2: Pointers To Everything](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-2-pointers.html)
