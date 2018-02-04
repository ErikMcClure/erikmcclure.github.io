+++
title = "C# to C++ Tutorial - Part 3: Classes and Structs and Inheritance (OH MY!)"
date = 2011-09-09T18:07:00Z
updated = 2013-05-10T02:01:38Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

[ [1](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-1-basics-of-syntax.html) &middot; [2](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-2-pointers.html) &middot; **3** &middot; [4](http://blackhole12.blogspot.com/2012/10/c-to-c-tutorial-part-4-operator-overload.html) {{<span style="color:#aaa">}}&middot; 5 &middot; 6 &middot; 7{{</span>}} ]

Classes in C#, like most object-oriented languages, are very similar to their C++ counterparts. They are declared with {{<code>}}class{{</code>}}, exist between brackets and inherit classes using a colon {{<code>}}':'{{</code>}}. Note, however, that all classes in C++ **must end with a semicolon!** You will forget this semicolon, and then *all the things will break*. You can do pretty much everything you can do with a C# class in a C++ class, except that C++ does not have {{<code>}}partial{{</code>}} classes, and in C++ classes themselves cannot be declared {{<code>}}public{{</code>}}, {{<code>}}protected{{</code>}} or {{<code>}}private{{</code>}}. Both of these features don't exist because they are made irrelevant with how classes are declared in header files.

In C# you usually just have one code file with the class declared in it along with all the code for all the functions. You can just magically use this class everywhere else and everything is fun and happy with rainbows. As mentioned before, C++ uses header files, and they are heavily integrated into the class system. We saw before how in order to use a function somewhere else, its prototype must first be declared in the header file. This applies to both classes and pretty much everything else. You need to understand that unlike C#, C++ does not have magic dust in its compiler. In C++, it just goes down the list of .cpp files, does a bit of dependency optimization, and then simply compiles each .cpp file by taking all the content from all the headers that are included (including all the headers included in the headers) and pasting it before the actual code from the .cpp file, and compiling. This process is repeated separately for every single code file, and no order inconsistencies are allowed anywhere in the code, the headers, *or even the order that the headers are included in*. The compiler literally takes every single #include statement as it is and simply replaces it with the code of the header it points to, wherever this happens to be in the code. This can (and this has happened to me) result in certain configurations of header files working even though one header file is actually missing a dependency. For example:
{{<pre cpp>}}//Rainbow.h

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
}; // DO NOT FORGET THE SEMICOLON
{{</pre>}}{{<pre cpp>}}//Unicorn.h

class Unicorn
{
  int magic;
};
{{</pre>}}{{<pre cpp>}}//main.cpp

#include "Unicorn.h"
#include "Rainbow.h"

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}
Compiling main.cpp will succeed in this case, even though Rainbow.h is referencing the {{<code>}}Unicorn{{</code>}} class without it ever being declared. The reason behind this is what happens when the compiler expands all the includes. Right before compiling main.cpp (after the preprocessor has run), main.cpp looks like this:
{{<pre cpp>}}//main.cpp

//Unicorn.h

class Unicorn
{
  int magic;
};
//Rainbow.h

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
}; // DO NOT FORGET THE SEMICOLON

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}
It is now obvious that because Rainbow.h was included after Unicorn.h, the {{<code>}}Unicorn{{</code>}} reference was resolved since it was declared before {{<code>}}Rainbow{{</code>}}. However, had we reversed the order of the include files, we would have had an anachronism: an inconsistency in our chronological arrangement. It is very bad practice to construct headers that are dependent on the order in which they are included, so we usually resolve something like this by having Rainbow.h simply include Unicorn.h, and then it won't matter what order they are included in.
{{<pre cpp>}}//Rainbow.h

#include "Unicorn.h"

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
{{</pre>}}Left as is, however, and we run into a problem. Lets try compiling main.cpp:  {{<pre cpp>}}//main.cpp

#include "Rainbow.h"
#include "Unicorn.h"

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}{{<pre cpp>}}//main.cpp

//Rainbow.h

#include "Unicorn.h"

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
//Unicorn.h

class Unicorn
{
  int magic;
};

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}{{<pre cpp>}}//main.cpp

//Rainbow.h

//Unicorn.h

class Unicorn
{
  int magic;
};

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
//Unicorn.h

class Unicorn
{
  int magic;
};

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}
We've just declared {{<code>}}Unicorn{{</code>}} twice! Obviously one way to solve this in our very, very simplistic example is to just remove the spurious {{<code>}}#include{{</code>}} statement, but this violates the unwritten rule of header files - any header file should be able to be included anywhere in any order regardless of what other header files have been included. This means that, first, any header file should include all the header files that it needs to resolve its dependencies. However, as we see here, that simply makes it extremely likely that a header file will get included 2 or 3 or maybe hundreds of times. What we need is an **include guard**.
{{<pre cpp>}}//Unicorn.h

#ifndef __UNICORN_H__
#define __UNICORN_H__

class Unicorn
{
  int magic;
};

#endif
{{</pre>}}
Understanding this requires knowledge of the **C Preprocessor**, which is what goes through and *processes* your code before its compiled. It is very powerful, but right now we only need to know the basics. Any statement starting with {{<code>}}#{{</code>}} is a preprocessor command. You will notice that {{<code>}}#include{{</code>}} is itself a preprocessor command, which makes sense, since the preprocessor was replacing those {{<code>}}#include{{</code>}}'s with the code they contained. {{<code>}}#define{{</code>}} lets you *define* a constant (or if you want to be technical, an *object-like macro*). It can be equal to a number or a word or just not be equal to anything and simply be in a *defined state*. {{<code>}}#ifdef{{</code>}} and {{<code>}}#endif{{</code>}} are just an if statement that allows the code inside of it to exist if the given constant is *defined*. {{<code>}}#ifndef{{</code>}} simply does the opposite - the code inside only exists if the given constant *doesn't* exist.

So, what we do is pick a constant name that probably will never be used in anything else, like {{<code>}}__UNICORN_H__{{</code>}}, and put in a check to see if it is defined. The first time the header is reached, it won't be defined, so the code inside {{<code>}}#ifndef{{</code>}} will exist. The next line tells the preprocessor to define{{<code>}} __UNICORN_H__{{</code>}}, the constant we just checked for. That means that the next time this header is included, {{<code>}}__UNICORN_H__{{</code>}} will have been defined, and so the code will be skipped over. Observe:
{{<pre cpp>}}//main.cpp

#include "Rainbow.h"
#include "Unicorn.h"

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}{{<pre cpp>}}//main.cpp

//Rainbow.h

#include "Unicorn.h"

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
//Unicorn.h

#ifndef __UNICORN_H__
#define __UNICORN_H__

class Unicorn
{
  int magic;
};

#endif

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}{{<pre cpp>}}//main.cpp

//Rainbow.h

//Unicorn.h

#ifndef __UNICORN_H__
#define __UNICORN_H__

class Unicorn
{
  int magic;
};

#endif

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
//Unicorn.h

#ifndef __UNICORN_H__
#define __UNICORN_H__

class Unicorn
{
  int magic;
};

#endif

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}{{<pre cpp>}}//main.cpp

//Rainbow.h

//Unicorn.h

#ifndef __UNICORN_H__
#define __UNICORN_H__

class Unicorn
{
  int magic;
};

#endif

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
//Unicorn.h

#ifndef __UNICORN_H__
#endif

int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}{{<pre cpp>}}//main.cpp

//Rainbow.h

//Unicorn.h


class Unicorn
{
  int magic;
};


class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};
//Unicorn.h


int main(int argc, char *argv[]) 
{
  Rainbow rainbow;
}
{{</pre>}}
Our problem is solved! However, note that {{<code>}}//Unicorn.h{{</code>}} was left in, because it was outside the include guard. It is **absolutely critical** that you put *everything* inside your include guard (ignoring comments), or it will either not work properly or be extremely inefficient.
{{<pre cpp>}}//Rainbow.h

#include "Unicorn.h"

#ifndef __RAINBOW_H__ //WRONG WRONG WRONG WRONG WRONG
#define __RAINBOW_H__

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};

#endif
{{</pre>}}
In this case, the code still *compiles*, because the include guards prevent duplicate definitions, but its very taxing on the preprocessor that will repeatedly attempt to include {{<code>}}Unicorn.h{{</code>}} only to discover that it must be skipped over anyway. The preprocessor may be powerful, but it is also **very dumb** and is easily crippled. The thing is slow enough as it is, so try to keep its workload to a minimum by putting your {{<code>}}#include{{</code>}}'s inside the include guard. Also, don't put semicolons on preprocessor directives. Even though almost everything else in the entire language wants semicolons, semicolons in preprocessor directives will either be redundant or considered a syntax error.
{{<pre cpp>}}//Rainbow.h

#ifndef __RAINBOW_H__
#define __RAINBOW_H__

#include "Unicorn.h" // SMILES EVERYWHERE!

class Rainbow
{
  Unicorn _unicorns[5]; // 5 unicorns dancing on rainbows
};

#endif
{{</pre>}}
Ok, so now we know how to properly use header files, but not how they are used to declare classes. Let's take a class declared in C#, and then transform it into an equivalent prototype in C++.
{{<pre csharp>}}public class Pegasus : IComparable<Pegasus>
{
  private Rainbow rainbow;
  protected int magic;
  protected bool flying;

  const int ID=10;
  static int total=0;
  const string NAME="Pegasus";

  public Pegasus()
  {
    flying=false;
    magic=1;
    IncrementTotal();
  }
  ~Pegasus()
  {
    magic=0;
  }
  public void Fly()
  {
    flying=true;
  }
  private void Land()
  {
    flying=false;
  }
  public static string GetName()
  {
    return NAME;
  }
  private static void IncrementTotal()
  {
    ++total;
  }
  public int CompareTo(Pegasus other)
  {
    return 0;
  }
}
{{</pre>}}{{<pre cpp>}}class Pegasus : public IComparable<Pegasus>
{
public:
  Pegasus();
  ~Pegasus();
  void Fly();
  virtual int CompareTo(Pegasus& other);
  
  static const int ID=10;
  static int total;
  static const char* NAME;

  inline static void IncrementTotal() { ++total; }

protected:
  int magic;
  bool flying;

private:
  void Land();
  
  Rainbow rainbow;
};
{{</pre>}}
Immediately, we are introduced to C++'s method of dealing with {{<code>}}public{{</code>}}, {{<code>}}protected{{</code>}} and {{<code>}}private{{</code>}}. Instead of specifying it for each item, they are done in groups. The inheritance syntax is identical, and we've kept the static variables, but now only one of them is being initialized in the class. In C++, you cannot initialize a static variable inside a class unless it is a {{<code>}}static const int{{</code>}} (or any other integral type). Instead, we will have to initialize {{<code>}}total{{</code>}} and {{<code>}}NAME{{</code>}} when we get around to implementing the code for this class. In addition, while most of the functions do not have code, as expected, {{<code>}}IncrementTotal{{</code>}} does. As an aside, C# does not have {{<code>}}static const{{</code>}} because it considers it redundant - all constant values are static. C++, however, allows you to declare a {{<code>}}const{{</code>}} variable that isn't static. While this would be useless in C#, there are certain situations where it is useful in C++.

If a given function's code doesn't have any dependencies unavailable in the header file the class is declared in, you can define that method in the class prototype itself. However, [as I mentioned before](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-1-basics-of-syntax.html#header-code), code in header files runs the danger of being compiled twice. While the compiler is usually good about properly instancing the class, it is usually a good idea to {{<code>}}inline{{</code>}} any functions defined in the header. Functions that are {{<code>}}inline{{</code>}}'d are embedded inside code that calls them instead of being explicitly called. That means instead of pushing arguments on to the stack and returning, the compiler simply embeds the function inside of the code that called it, like so:
{{<pre cpp>}}#include "Pegasus.h"

// Before compilation
int main(int argc, char *argv[]) 
{
  Pegasus::IncrementTotal()
}

// After compilation
int main(int argc, char *argv[]) 
{
  ++Pegasus::total;
}
{{</pre>}}
The consequence of this means that the function itself is never actually instantiated. In fact the function might as well not exist - you won't be able to call it from a DLL because the function was simply embedded everywhere that it was used, kind of like a fancy macro. This neatly solves our issue with code in header files, and will be important later on. This also demonstrates how one accesses static variables and functions in a class. Just like before, the C# method of using {{<code>}}.{{</code>}} no longer works, you must use the Scope Resolution Operator ({{<code>}}::{{</code>}}) to access static members and functions of a class. This same operator is what allows us to declare the code elsewhere without confusing the compiler.
{{<pre cpp>}}//Pegasus.cpp

#include "Pegasus.h"

int Pegasus::total = 0;
const char* Pegasus::NAME = "Pegasus";

Pegasus::Pegasus() : IComparable<Pegasus>(), magic(1), flying(false)
{
  IncrementTotal();
}

Pegasus::~Pegasus()
{
  magic=0;
}

void Pegasus::Fly()
{
  flying=true;
}

void Pegasus::Land()
{
  flying=false;
}

string Pegasus::GetName()
{
  return NAME;
}

int Pegasus::CompareTo(Pegasus other)
{
  return 0;
}
{{</pre>}}
This looks similar to what our C# class looked like, except the functions aren't in the class anymore. {{<code>}}Pegasus::{{</code>}} tells the compiler what class the function you are defining belongs in, which allows it to assign the class function prototype to the correct implementation, just like it did with normal functions before. Notice that {{<code>}}static{{</code>}} is not used when defining {{<code>}}GetName(){{</code>}} - All function decorations ({{<code>}}inline{{</code>}}, {{<code>}}static{{</code>}}, {{<code>}}virtual{{</code>}}, {{<code>}}explicit{{</code>}}, etc.) are **only allowed on the function prototype**. Note that all these rules apply to static variable initialization as well; both {{<code>}}total{{</code>}} and {{<code>}}NAME{{</code>}} are resolved using {{<code>}}Pegasus::{{</code>}} and **don't** have the {{<code>}}static{{</code>}} decorator, only their type. Even though we're using {{<code>}}const char*{{</code>}} instead of {{<code>}}string{{</code>}}, you can still initialize a constant value using {{<code>}}= "string"{{</code>}}.

The biggest difference here is in the constructor. In C#, the only things you bother with in the constructor after the colon is either initializing a subclass or calling another constructor. In C++, you can initialize any subclasses you have along with any variables you have, including passing arguments to whatever constructors your variables might have. Most notably is the ability to initialize constant values, which means you can have a constant integer that is set to a value passed through the constructor, or based off a function call from somewhere else. Unfortunately, C++ traditionally does not allow initializing any variables in any sub-classes, nor does it allow calling any of your own constructors. C++0x [partially resolves this problem](http://en.wikipedia.org/wiki/C%2B%2B0x#Object_construction_improvement), but it is not fully implemented in VC++ or other modern compilers. This blow, however, is mitigated by default arguments in functions (and by extension, constructors), which allows you to do more with fewer functions.

The order in which variables are constructed is occasionally important if there is an inter-dependency between them. While having such inter-dependencies are generally considered a bad idea, they are sometimes unavoidable, and you can take advantage of a compiler's default behavior of initializing the values in a left to right order. While this behavior isn't technically guaranteed, it is sufficiently reliable for you to take use of it in the occasional exceptional case, but always double-check that the compiler hasn't done crazy optimization in the release version (usually, though, this will just blow the entire program up, so it's pretty obvious).

Now, C# has another datatype, the {{<code>}}struct{{</code>}}. This is a limited datatype that cannot have a constructor and is restricted to value-types. It is also passed by-value through functions by default, unlike classes. This is very similar to how structs behaved in C, but have no relation to C++'s {{<code>}}struct{{</code>}} type. In C++, a {{<code>}}struct{{</code>}} is *completely identical* to a {{<code>}}class{{</code>}} in every way, save for one minor detail: all members of a {{<code>}}class{{</code>}} are {{<code>}}private{{</code>}} by default, while all members of a {{<code>}}struct{{</code>}} are {{<code>}}public{{</code>}} by default. That's it. You can take *any* {{<code>}}class{{</code>}} and replace it with {{<code>}}struct{{</code>}} and the only thing that will change is the default access modifier.

Even though there is no direct analogue to C#'s {{<code>}}struct{{</code>}}, there is an implicit equivalent. If a {{<code>}}class{{</code>}} or {{<code>}}struct{{</code>}} (C++ really doesn't care) meets the requirements of a traditional C {{<code>}}struct{{</code>}} (no constructor, only basic data types), then it's treated as Plain Old Data, and you are then allowed to skip the constructor and initialize its contents using the special bracket initialization that was [touched on before](http://blackhole12.blogspot.com/2011/07/c-to-c-tutorial-part-2-pointers.html#struct-init). Yes, you can initialize constant variables using that syntax too.

One thing I've skipped over is the {{<code>}}virtual{{</code>}} code decorator in the C++ prototype of {{<code>}}Pegasus{{</code>}}, which is not actually necessary, because the function is already attempting to override another virtual function declared in {{<code>}}IComparable{{</code>}}, which implicitly makes it virtual. However, in C#, {{<code>}}IComparable{{</code>}} is implemented as an {{<code>}}interface{{</code>}}, which is not present in C++. Of course, if you really think about it, an {{<code>}}interface{{</code>}} is kind of like a normal class, just with all {{<code>}}abstract{{</code>}} methods (ignore the inheritance issues with this for now). So, we could rewrite the C# implementation of {{<code>}}IComparable{{</code>}} as a class with abstract methods:
{{<pre csharp>}}public class IComparable<T>
{
  public abstract int CompareTo(T other);
}
{{</pre>}}
As it turns out, this has a direct C++ analogue:  
{{<pre csharp>}}template<class T>
class IComparable
{
public:
  virtual int CompareTo(T other)=0;
}
{{</pre>}}
This {{<code>}}virtual{{</code>}} function, instead of being implemented, has an {{<code>}}=0{{</code>}} on the end of it. That makes the function *pure virtual*, which is just another way of saying {{<code>}}abstract{{</code>}}. So the C++ version of {{<code>}}abstract{{</code>}} is a pure virtual function, and a C++ version of interfaces is just a class made entirely out of pure virtual functions. Just as C# prevents you from instantiating an {{<code>}}abstract class{{</code>}} or {{<code>}}interface{{</code>}}, C++ considers any class that either declares or inherits pure virtual functions without giving them code as an {{<code>}}abstract class{{</code>}} that cannot be instantiated. Unfortunately C++ does not have anything like {{<code>}}sealed{{</code>}}, {{<code>}}override{{</code>}}, etc., so you are on your own there. Keep in mind that {{<code>}}public IComparable<T>{{</code>}} could easily be replaced with {{<code>}}protected{{</code>}} or {{<code>}}private{{</code>}} for more control.

The reason C# has interfaces at all is because C# only allows you to inherit a single class, regardless of whether or not its abstract. If its got code, you can only inherit it once. Interfaces, however, have no code, and so C# lets you pile them on like candy. This isn't done in C++, because C++ supports multiple inheritance. In C++ you can have any class inherit any other class, no matter what, but you can only instantiate a class if it provides implementations for all pure virtual functions somewhere along its inheritance line. Unfortunately, there are a lot of caveats about multiple inheritance, the most notorious being [the Diamond Problem](http://en.wikipedia.org/wiki/Diamond_problem).

Let's say you have a graphics engine that has an {{<code>}}Image{{</code>}} class, and that image class inherits from an {{<code>}}abstract class{{</code>}} that holds its position. Obviously, any image on the screen is going to have a position. Then, let's take a physics engine, with a basic object that also inherits from an abstract class that holds its position. Obviously any physics object must have a position. So, what happens when you have a game object that is both an image and a physics object? Since the image and the physics object are in fact the same thing, both of them must have the same position at all times, but both inherit the abstract class storing position separately, resulting in two positions. Which one is the right position? When you call SetPosition, which position are you talking about?

Virtual inheritance was introduced as an attempt to solve this problem. It works by creating a single instance of a derived class for the entire inheritance change, such that both the physics object and the image share the same position, as they are supposed to. Unfortunately, it can't resolve all the ambiguities, and it introduces a whole truckload of new problems. It has a nasty habit of being unable to resolve its own virtual functions properly and introducing all sorts of horrible weirdness. Most incredibly bizarre is a virtually inherited class's constructor - it must be initialized in the last class in the inheritance chain, and is one of the first classes to get its constructor called, regardless of where it might be in the hierarchy. It's destructor order is equally as bizarre. Virtual inheritance is sometimes useful for certain small utility classes that must be shared through a wide variety of situations, like a flag class. As a rule of thumb, you should only use virtual inheritance in a class that either relies on the default constructor or only offers a constructor that takes no arguments, and has no superclasses. This allows you to just slap the {{<code>}}virtual{{</code>}} keyword on and forget about all the wonky constructor details.
{{<pre cpp>}}class Pegasus : virtual IComparable<Pegasus>{{</pre>}}
If you ever think you need to use virtual inheritance on something more complicated, your code is broken and you need to rethink your program's architecture (and the compiler probably won't be able to do it properly anyway). On a side-note, the constructors for any given object are called from the top down. That is, when your object's constructor is called, it immediately calls all the constructors for all it's superclasses, usually before even doing any variable initialization, and then those object constructors immediately call all *their* superclass constructors, and so on until the first line of code executed in your program is whatever the topmost class was. This then filters down until control is finally returned to your original constructor, such that any constructor code is only executed *after* all of its base classes have been constructed. The exact reverse happens for destructors, with the lowest class destructor being executed first, and after its finished, the destructors for all its base classes are called, such that a class destructor is always called while all of its base classes still exist.

Hopefully you are familiar with C#'s {{<code>}}enum{{</code>}} keyword. While it used to be far more limited, it has now been extended to such a degree it is *identical* to C++, even the syntax is the same. The only difference between the two is that the C++ version can't be declared {{<code>}}public{{</code>}}, {{<code>}}protected{{</code>}} or {{<code>}}private{{</code>}} and needs to have a semicolon on the end (like everything else). Like in C#, enums, classes and structs can be embedded in classes, except in C++ they can also be embedded in structs (because structs are basically classes with a different name). Also, C++ allows you to declare an {{<code>}}enum{{</code>}}/{{<code>}}class{{</code>}}/etc. and a variable inside the class at the same time using the following syntax:
{{<pre cpp>}}class Pegasus
{
  enum Count { Uno=2, Dos, Tres, Quatro, Cinco } variable;
  enum { Uno=2, Dos, Tres, Quatro, Cinco } var2; //When used to immediately declare a variable, enums can be anonymous
}

//Same as above
class Pegasus
{
  enum Count { Uno=2, Dos, Tres, Quatro, Cinco }; //cannot be anonymous

  Count variable;
  Count var2;
}
{{</pre>}}
Unions are exclusive to C++, and are a special kind of data structure where each element occupies the same address. To understand what that means, let's look at an example:
{{<pre cpp>}}union //Unions are usually anonymous, but can be named
{
  struct { // The anonymity of this struct exposes its internal members.
    __int32 low;
    __int32 high;
  }
  __int64 full;
}
{{</pre>}}union  
{{<code>}}__int32{{</code>}} and {{<code>}}__int64{{</code>}} are simply explicitly declaring 32-bit and 64-bit integers. This {{<code>}}union{{</code>}} allows us to either set an entire 64-bit integer, or to only set its low or high portion. This happens because the data structure is laid out as follows:

{{<img src="http://img705.imageshack.us/img705/2389/p3union1.png" alt="Integer Union Layout" >}}

Both {{<code>}}low{{</code>}} and {{<code>}}full{{</code>}} are mapped to the *exact same place in memory*. The only difference is that {{<code>}}low{{</code>}} is a 32-bit integer, so when you set that to 0, only the first four bytes are set to zero. {{<code>}}high{{</code>}} is pointing to a location in memory that is exactly 4 bytes in front of {{<code>}}low{{</code>}} and {{<code>}}full{{</code>}}. So, if {{<code>}}low{{</code>}} and {{<code>}}full{{</code>}} were located at {{<code>}}0x000F810{{</code>}}, {{<code>}}high{{</code>}} would be located at {{<code>}}0x000F814{{</code>}}. Setting {{<code>}}high{{</code>}} to zero sets the last four bytes to zero, but doesn't touch the first four. Consequently, if you set {{<code>}}high{{</code>}} to 0, reading {{<code>}}full{{</code>}} would always return the same value as {{<code>}}low{{</code>}}, since it would essentially be constrained to a 32-bit integer. Unions, however, do not have to have matching memory layouts:
{{<pre cpp>}}union //Unions are usually anonymous, but can be named
{
  char pink[5]
  __int32 fluffy;
  __int64 unicorns;
}
{{</pre>}}
The layout of this union is:

{{<img src="http://img851.imageshack.us/img851/1847/p3union2.png" alt="Jagged Union Layout" >}}

Any unused space is simply ignored. This same rule would apply for any structs being used to group data. The size of the {{<code>}}union{{</code>}} is simply the size of its largest member. Setting all 5 elements of {{<code>}}pink{{</code>}} here would result in {{<code>}}fluffy{{</code>}} being equal to zero, and only the last 24-bits (or last 3 bytes) of {{<code>}}unicorns{{</code>}} be untouched. Likewise, setting {{<code>}}fluffy{{</code>}} to zero would zero out the first 4 elements in {{<code>}}pink{{</code>}} (indexes 0-3), leaving the 5{{<sup>}}th{{</sup>}} untouched. These unions are often used in performance critical areas where a single function must be able to recieve many kinds of data, but will only ever recieve a single group of data at a time, and so it would be more efficient to map all the possible memory configurations to a single data structure that is large enough to hold the largest group. Here is a real world example:
{{<pre cpp>}}struct __declspec(dllexport) cGUIEvent
{
  cGUIEvent() { memset(this,0,sizeof(cGUIEvent)); }
  cGUIEvent(unsigned char _evt, const cVecT<int>* mousecoords, unsigned char _button, bool _pressed) : evt(_evt), subevt(0), mousecoords(mousecoords), button(_button), pressed(_pressed) {}
  cGUIEvent(unsigned char _evt, const cVecT<int>* mousecoords, unsigned short _scrolldelta) : evt(_evt), subevt(0), mousecoords(mousecoords), scrolldelta(_scrolldelta) {}
  union
 {
  struct
  {
   unsigned char evt;
   unsigned char subevt;
  };
  unsigned short realevt;
 };

  union
  {
    struct { const cVecT<int>* mousecoords; unsigned char button; bool pressed; };
    struct { const cVecT<int>* mousecoords; short scrolldelta; };
    struct { //the three ctrl/shift/alt bools (plus a held bool) here are compressed into a single byte
      bool down;
      unsigned char keycode; //only used by KEYDOWN/KEYUP
      char ascii; //Only used by KEYCHAR
      wchar_t unicode; //Only used by KEYCHAR
      char sigkeys;
    };
    struct { float value; short joyaxis; }; //JOYAXIS
    struct { bool down; short joybutton; }; //JOYBUTTON*
  };
};
{{</pre>}}
Here, the GUI event is mapped to memory according to the needs of the event that it is representing, without the need for complex inheritance or wasteful memory usage. Unions are indispensable in such scenarios, and as a result are very common in any sort of message handling system.

One strange decorator that has gone unexplained in the above example is the {{<code>}}__declspec(dllexport){{</code>}} class decorator. When creating a windows DLL, if you want anything to be usable by something inheriting the DLL, you have to export it. In VC++, this can be done with a [module definition file (.def)](http://msdn.microsoft.com/en-us/library/28d6s79h(v=vs.80).aspx), which is useful if you'll be using GetProcAddress manually, but if you are explicitly linking to a DLL, {{<code>}}__declspec(dllexport){{</code>}} automatically exports the function for you when placed on a function. When placed on a class, it automatically exports the entire class. However, for anyone to utilize it, they have to have the header file. This arises to DLLs being distributed as DLLs, linker libraries (.lib), and sets of header files, usually in an "include" directory. In certain cases, only some portions of your DLL will be accessible to the outside, and so you'll want two collections of header files - outside header files and internal ones that no one needs to know about. Consequently, utilizing a large number of C++ DLLs usually involves substantial organization of a whole lot of header files.

Due to the compiler-specific nature of DLL management, they will be covered in [Part 6](#). For now, its on to operator overloading, copy semantics and move semantics!

[Part 4: Operator Overload](http://blackhole12.blogspot.com/2012/10/c-to-c-tutorial-part-4-operator-overload.html)

{{<html>}}<iframe width="640" height="390" src="http://www.youtube-nocookie.com/embed/eWM2joNb9NE?rel=0&hd=1" frameborder="0" allowfullscreen></iframe>{{</html>}}