+++
blogimport = true
categories = ["blog"]
comments = [7902195492669411275, 7885397258488716875, 8618545649754470268]
date = "2012-10-20T11:16:00Z"
title = "C# to C++ Tutorial - Part 4: Operator Overload"
updated = "2013-05-10T01:57:58.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
[ [1]({{< ref "c-to-c-tutorial-part-1-basics-of-syntax.md" >}}) &middot; [2]({{< ref "c-to-c-tutorial-part-2-pointers.md" >}}) &middot; [3]({{< ref "c-to-c-tutorial-part-3-classes-and.md" >}}) &middot; **4** &middot; <span style="color:#aaa;">5 &middot; 6 &middot; 7</span> ]

If you are familiar with C#, you should be familiar with the difference between C#'s {{<code>}}struct{{</code>}} and {{<code>}}class{{</code>}} declarations. Namely, a {{<code>}}struct{{</code>}} is a value type and a {{<code>}}class{{</code>}} is a reference type, meaning that if you pass a struct to a function, its default behavior is for the entire struct to be **copied** into the function's parameter, so any modifications made to it won't affect whatever was passed in. On the flip side, a class is a reference value, so a **reference** is passed into the function, and any changes made to that reference will be reflected in the object that was originally passed into the function.

{{<pre csharp>}}// Takes an integer, or a basic value type
public static int add(int v)
{
  v+=3;
  return 4+v;
}

public struct Oppa
{
  public string gangnam;
}

// Takes a struct, or a complex value type
public static Oppa style(Oppa g)
{
  g.gangnam="notstyle";
  return g;
}

public class Psy
{
  public int style;
}

// Takes a class, or a reference type
public static void change(Psy psy)
{
  psy.style=5;
}

// Takes an integer, but forces it to be passed by reference instead of by value.
public static int addref(ref int v)
{
  v+=3;
  return 4+v;
}

int a = 0;
int b = add(a);
// a is still 0
// b is now 7

int c = addref(a);
// a is now 3, because it was passed by reference
// c is now 7

Oppa s1;
s1.gangnam="style";
Oppa s2 = style(s1);
//s1.gangnam is still "style"
//s2.gangnam is now "notstyle"

Psy psy = new Psy();
psy.style=0;
change(psy);
// psy.style is now 5, because it was passed by reference
{{</pre>}}
C++ also lets you pass in parameters by reference and by value, however it is more explicit about what is happening, so there is no default behavior to know about. If you simply declare the type itself, for example {{<code>}}(myclass C, int B){{</code>}}, then it will be passed by value and copied. If, however, you use the reference symbol that we've used before in variable declarations, it will be passed by reference. This happens no matter what. If a reference is passed into a function that takes a value, it will still have a copy made.

{{<pre cpp>}}// Integer passed by value
int add(int v)
{
  v+=3;
  return 4+v;
}

class Psy
{
public:
  int style;
};

// Class passed by value
Psy change(Psy psy)
{
  psy.style=5;
  return psy;
}

// Integer passed by reference
int addref(int& v)
{
  v+=3;
  return 4+v;
}

// Class passed by reference
Psy changeref(Psy& psy)
{
  psy.style=5;
  return psy;
}

int horse = 2;
int korea = add(horse);
// horse is still 2
// korea is now 9

int horse2 = 2;
int korea2 = addref(horse2);
// horse2 is now 5
// korea2 is now 9

Psy psy;
psy.style = 0;
Psy ysp = change(psy);
// psy.style is still 0
// ysp.style is now 5

Psy psy2;
psy2.style = 0;
Psy ysp2 = changeref(psy2);
// psy2.style is now 5
// ysp2.style is also 5
{{</pre>}}
However, in order to copy something, C++ needs to know how to properly copy your class. This gives rise to the **copy constructor**. By default, the compiler will automatically generate a copy constructor for your class that simply invokes all the default copy constructors of whatever member variables you have, just like C#. If, however, your class is holding on to a pointer, then this is going to cause a giant mess when two classes are pointing to the same thing and one of the deletes what it's pointing to! By specifying a copy constructor, we can deal with the pointer properly:

{{<pre cpp>}}class myString
{
public:
  // The copy constructor, which copies the string over instead of copying the pointer
  myString(const myString& copy)
  {
    size_t len = strlen(copy._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,copy._str,sizeof(char)*len);
  }
  // Normal constructor
  myString(const char* str)
  {
    size_t len = strlen(str);
    _str=new char[len];
    memcpy(_str,str,sizeof(char)*len);
  }
  // Destructor that deallocates our string
  ~myString()
  {
    delete [] _str;
  }

private:
  char* _str;
};

{{</pre>}}
This copy constructor can be invoked manually, but it will simply be implicitly called whenever its needed. Of course, that isn't the only time we need to deal with our rogue pointer that screws things up. What happens when we set our class equal to another class? Remember, **a reference cannot be changed after it is created**. Observe the following behavior:

{{<pre cpp>}}int a = 3;
int b = 2;
int& ra = a;
int* pa = &a;

b = a; //b is now 3
a = 0; //b is still 3, but a is now 0
b = ra; // b is now 0
a = 5; // b is still 0 but now a is 5
b = *pa; // b is now 5
b = 8; // b is now 8 but a is still 5

ra = b; //a is now 8! This assigns b's values to ra, it does NOT change the reference!
ra = 9; //a is now 9, and b is still 8! ra STILL refers to a, and NOTHING can change that.

pa = &b; // Now pa points to to b
a = *pa; // a is now 8, because pointers CAN be changed.
*pa = 7; // Now b is 7, but a is still 8

int*& rpa = pa; //Now we have a reference to a pointer (C++11)
//rpa = 5; // COMPILER ERROR, rpa is a reference to a POINTER
int** ppa = &pa;
//rpa = ppa; // COMPILER ERROR, rpa is a REFERENCE to a pointer, not a pointer to a pointer!
rpa = &a; //now pa points to a again. This does NOT change the reference!
b = *pa; // Now b is 8, the same as a.
{{</pre>}}
So somehow, we have to overload the assignment operator! This brings us to **Operator Overloading**. [C# operator overloading](http://msdn.microsoft.com/en-us/library/aa288467(v=vs.71).aspx) works by defining *global* operator overloads, ones that take a left and a right argument, and are static functions. By default, C++ operator overloading only take the *right argument*. The left side of the equation is implied to be the class itself. Consequently, **C++ operators are not static**. C++ does have global operators, but they are defined **outside the class**, and the assignment operator isn't allowed as a global operator; you have to define it inside the class. All the overload-able operators are shown below with appropriate declarations:

{{<pre cpp>}}class someClass
{
someClass operator =(anything b); // me=other
someClass operator +(anything b); // me+other
someClass operator -(anything b); // me-other
someClass operator +(); // +me
someClass operator -(); // -me (negation)
someClass operator *(anything b); // me*other
someClass operator /(anything b); // me/other
someClass operator %(anything b); // me%other
someClass& operator ++(); // ++me
someClass& operator ++(int); // me++
someClass& operator --(); // --me
someClass& operator --(int); // me--
// All operators can TECHNICALLY return any value whatsoever, but for many of them only certain values make sense.
bool operator ==(anything b); 
bool operator !=(anything b);
bool operator >(anything b);
bool operator <(anything b);
bool operator >=(anything b);
bool operator <=(anything b);
bool operator !(); // !me
// These operators do not usually return someClass, but rather a type specific to what the class does.
anything operator &&(anything b); 
anything operator ||(anything b);

anything operator ~();
anything operator &(anything b);
anything operator |(anything b);
anything operator ^(anything b);
anything operator <<(anything b);
anything operator >>(anything b);
someClass& operator +=(anything b); // Should always return *this;
someClass& operator -=(anything b);
someClass& operator *=(anything b);
someClass& operator /=(anything b);
someClass& operator %=(anything b);
someClass& operator &=(anything b);
someClass& operator |=(anything b);
someClass& operator ^=(anything b);
someClass& operator <<=(anything b);
someClass& operator >>=(anything b);
anything operator [](anything b); // This will almost always return a reference to some internal array type, like myElement&
anything operator *();
anything operator &();
anything* operator ->(); // This has to return a pointer or some other type that has the -> operator defined.

anything operator ->*(anything a);
anything operator ()(anything a1, U a2, ...);
anything operator ,(anything b);
operator otherThing(); // Allows this class to have an implicit conversion to type otherThing
void* operator new(size_t x); // These are called when you write new someClass()
void* operator new[](size_tx); // new someClass[num]
void operator delete(void*x); // delete pointer_to_someClass
void operator delete[](void*x); // delete [] pointer_to_someClass

};

// These are global operators that behave more like C# operators, but must be defined outside of classes, and a few operators do not have global overloads, which is why they are missing from this list. Again, operators can technically take or return any value, but normally you only override these so you can handle some other type being on the left side.
someClass operator +(anything a, someClass b);
someClass operator -(anything a, someClass b);
someClass operator +(someClass a);
someClass operator -(someClass a);
someClass operator *(anything a, someClass b);
someClass operator /(anything a, someClass b);
someClass operator %(anything a, someClass b);
someClass operator ++(someClass a);
someClass operator ++(someClass a, int); // Note the unnamed dummy-parameter int - this differentiates between prefix and suffix increment operators.
someClass operator --(someClass a);
someClass operator --(someClass a, int); // Note the unnamed dummy-parameter int - this differentiates between prefix and suffix decrement operators.

bool operator ==(anything a, someClass b);
bool operator !=(anything a, someClass b);
bool operator >(anything a, someClass b);
bool operator <(anything a, someClass b);
bool operator >=(anything a, someClass b);
bool operator <=(anything a, someClass b);
bool operator !(someClass a);
bool operator &&(anything a, someClass b);
bool operator ||(anything a, someClass b);

someClass operator ~(someClass a);
someClass operator &(anything a, someClass b);
someClass operator |(anything a, someClass b);
someClass operator ^(anything a, someClass b);
someClass operator <<(anything a, someClass b);
someClass operator >>(anything a, someClass b);
someClass operator +=(anything a, someClass b);
someClass operator -=(anything a, someClass b);
someClass operator *=(anything a, someClass b);
someClass operator /=(anything a, someClass b);
someClass operator %=(anything a, someClass b);
someClass operator &=(anything a, someClass b);
someClass operator |=(anything a, someClass b);
someClass operator ^=(anything a, someClass b);
someClass operator <<=(anything a, someClass b);
someClass operator >>=(anything a, someClass b);
someClass operator *(someClass a);
someClass operator &(someClass a);

someClass operator ->*(anything a, someClass b);
someClass operator ,(anything a, someClass b);
void* operator new(size_t x);
void* operator new[](size_t x);
void operator delete(void* x);
void operator delete[](void*x);
{{</pre>}}
We can see that the assignment operator mimics the arguments of our copy constructor. For the most part, it does the exact same thing; the only difference is that existing values must be destroyed, an operation that should mostly mimic the destructor. We extend our previous class to have an assignment operator accordingly:

{{<pre cpp>}}class myString
{
public:
  // The copy constructor, which copies the string over instead of copying the pointer
  myString(const myString& copy)
  {
    size_t len = strlen(copy._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,copy._str,sizeof(char)*len);
  }
  // Normal constructor
  myString(const char* str)
  {
    size_t len = strlen(str);
    _str=new char[len];
    memcpy(_str,str,sizeof(char)*len);
  }
  // Destructor that deallocates our string
  ~myString()
  {
    delete [] _str;
  }

  // Assignment operator, does the same thing the copy constructor does, but also mimics the destructor by deleting _str. NOTE: It is considered bad practice to call the destructor directly. Use a Clear() method or something equivalent instead.
  myString& operator=(const myString& right)
  {
    delete [] _str;
    size_t len = strlen(right._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,right._str,sizeof(char)*len);
  }

private:
  char* _str;
};
{{</pre>}}
These operations take an instance of the class and copy it's values to our instance. Consequently, these are known as *copy semantics*. If this was 1998, we'd stop here, because for a long time, C++ only had copy semantics. Either you passed around references to objects, or you copied them. You could also pass around pointers to objects, but remember that pointers are value types just like integers and floats, so you are really just copying them around too. In fact, until recently, you were *not allowed to have references to pointers*. Pointers were the one data type that had to be passed by value. Provided you are using a C++0x-compliant compiler, **this is no longer true**, as you may remember from our [first examples](). The new standard [released in 2011](en.wikipedia.org/wiki/C%2B%2B11) allows references to pointers, and introduces *move semantics*.

Move semantics are designed to solve the following problem. If we have a series of dynamic string objects being concatenated, with normal copy constructors we run into a serious problem:

{{<pre cpp>}}std::string result = std::string("Oppa") + std::string(" Gangnam") + std::string(" Style") + std::string(" by") + std::string(" Psy");
// This is evaluated by first creating a new string object with its own memory allocation, then deallocating both " by" and " Psy" after copying their contents into the new one
//std::string result = std::string("Oppa") + std::string(" Gangnam") + std::string(" Style") + std::string(" by Psy");
// Then another new object is made and " by Psy" and " Style" are deallocated
//std::string result = std::string("Oppa") + std::string(" Gangnam") + std::string(" Style by Psy");
// And so on and so forth
//std::string result = std::string("Oppa") + std::string(" Gangnam Style by Psy");
//std::string result = std::string("Oppa Gangnam Style by Psy");
// So just to add 5 strings together, we've had to allocate room for 5 additional strings in the middle of it, 4 of which are then simply deallocated!
{{</pre>}}
This is terribly inefficient; it would be much more efficient if we could utilize the temporary objects that are going to be destroyed anyway instead of reallocating a bunch of memory over and over again only to delete it immediately afterwards. This is where move semantics come in to play. First, we need to define a "temporary" object as one whose scope is entirely contained on *the right side of an expression*. That is to say, given a single assignment statement {{<code>}}a=b{{</code>}}, if an object is both created and destroyed inside {{<code>}}b{{</code>}}, then it is considered temporary. Because of this, these temporary values are also called *rvalues*, short for "right values". C++0x introduces the syntax {{<code>}}variable&&{{</code>}} to designate an rvalue. This is how you declare a move constructor:

{{<pre cpp>}}class myString
{
public:
  // The copy constructor, which copies the string over instead of copying the pointer
  myString(const myString& copy)
  {
    size_t len = strlen(copy._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,copy._str,sizeof(char)*len);
  }
  // Move Constructor
  myString(myString&& mov)
  {
    _str = mov._str;
    mov._str=NULL;
  }
  // Normal constructor
  myString(const char* str)
  {
    size_t len = strlen(str);
    _str=new char[len];
    memcpy(_str,str,sizeof(char)*len);
  }
  // Destructor that deallocates our string
  ~myString()
  {
    if(_str!=NULL) // Make sure we only delete _str if it isn't NULL!
      delete [] _str;
  }

  // Assignment operator, does the same thing the copy constructor does, but also mimics the destructor by deleting _str. NOTE: It is considered bad practice to call the destructor directly. Use a Clear() method or something equivalent instead.
  myString& operator=(const myString& right)
  {
    delete [] _str;
    size_t len = strlen(right._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,right._str,sizeof(char)*len);
    return *this;
  }

private:
  char* _str;
};{{</pre>}}**NOTE: Observe that our destructor functionality was changed! Now that _str can be NULL, we have to check for that before deleting the object.**

The idea behind a move constructor is that, instead of *copying* the values into our object, we *move* them into our object, setting the source to some {{<code>}}NULL{{</code>}} value. Notice that this can only work for pointers, or objects containing pointers. Integers, floats, and other similar types can't really be "moved", so instead their values are simply copied over. Consequently, move semantics is only beneficial for types like strings that involve dynamic memory allocation. However, because we must set the source pointers to 0, that means we can't use {{<code>}}const myString&&{{</code>}}, because then we wouldn't be able to modify the source pointers! This is why a move constructor is declared without a const modifier, which makes sense, since we intend to modify the object.

But wait, just like a copy constructor has an assignment copy operator, a move constructor has an equivalent assignment move operator. Just like the copy assignment, the move operator behaves exactly like the move constructor, but must destroy the existing object beforehand. The assignment move operator is declared like this:

{{<pre cpp>}}myString& operator=(myString&& right)
  {
    delete [] _str;
    _str=right._str;
    right._str=0;
    return *this;
  }
{{</pre>}}

Move semantics can be used for some interesting things, like unique pointers, that *only* have move semantics - by disabling the copy constructor, you can create an object that is impossible to copy, and can therefore only be moved, which guarantees that there will only be one copy of its contents in existence. {{<code>}}std::unique_ptr{{</code>}} is an implementation of this provided in C++0x. Note that if a data structure requires copy semantics, {{<code>}}std::unique_ptr{{</code>}} will throw a compiler error, instead of simply mysteriously failing like the deprecated {{<code>}}std::autoptr{{</code>}}.

There is an important detail when you are using inheritance or objects with move semantics:

{{<pre cpp>}}class Substring : myString
{
  Substring(Substring&& mov) : myString(std::move(mov))
  {
    _sub = std::move(mov._sub);
  }

  Substring& operator=(Substring&& right)
  {
    myString::operator=(std::move(right));
    _sub = std::move(mov._sub);
    return *this;
  }

  myString _sub;
};
{{</pre>}}
Here we are using {{<code>}}std::move(){{</code>}}, which takes a variable (that is either an rvalue or a normal reference) and returns an rvalue for that variable. This is because rvalues *stop being rvalues* the instant they are passed into a different function, which makes sense, since they are no longer on the right-hand side anymore. Consequently, if we were to pass {{<code>}}mov{{</code>}} above into our base class, it would trigger the *copy constructor*, because {{<code>}}mov{{</code>}} would be treated as {{<code>}}const Substring&{{</code>}}, instead of {{<code>}}Substring&&{{</code>}}. Using {{<code>}}std::move{{</code>}} lets us pass it in as {{<code>}}Substring&&{{</code>}} and properly trigger the move semantics. As you can see in the example, you must use {{<code>}}std::move{{</code>}} when moving any complex object, using base class constructors, or base class assignment operators. Note that {{<code>}}std::move{{</code>}} allows you to force an object to be moved to another object regardless of whether or not its actually an rvalue. This would be particularly useful for moving around {{<code>}}std::unique_ptr{{</code>}} objects.

There's some other weird things you can do with move semantics. This most interesting part is the strange behavior of {{<code>}}&&{{</code>}} when it is appended to existing references.

 * {{<code>}}A& &{{</code>}} becomes {{<code>}}A&{{</code>}}
 * **{{<code>}}A& &&{{</code>}} becomes {{<code>}}A&{{</code>}}**
 * {{<code>}}A&& &{{</code>}} becomes {{<code>}}A&{{</code>}}
 * **{{<code>}}A&& &&{{</code>}} becomes {{<code>}}A&&{{</code>}}**

By taking advantage of the second and fourth lines, we can perform *perfect forwarding*. Perfect forwarding allows us to pass an argument as either a normal reference ({{<code>}}A&{{</code>}}) or an rvalue ({{<code>}}A&&{{</code>}}) and then *forward it* into another function, preserving its status as an rvalue or a normal reference, including whether or not it's {{<code>}}const A&{{</code>}} or {{<code>}}const A&&{{</code>}}. Perfect forwarding can be implemented like so:

{{<pre cpp>}}template<typename U>
void Set(U && other)
{
  _str=std::forward<U>(other);
}
{{</pre>}}
Notice that this allows us to assign our data object using either the copy assignment, or the move assignment operator, by using {{<code>}}std::forward<U>(){{</code>}}, which transforms our reference into either an rvalue if it was an rvalue, or a normal reference if it was a normal reference, much like {{<code>}}std::move(){{</code>}} transforms everything into an rvalue. However, this requires a template, which may not always be correctly inferred. A more robust implementation uses two separate functions forwarding their parameters into a helper function:

{{<pre cpp>}}class myString
{
public:
  // The copy constructor, which copies the string over instead of copying the pointer
  myString(const myString& copy)
  {
    size_t len = strlen(copy._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,copy._str,sizeof(char)*len);
  }
  // Move Constructor
  myString(myString&& mov)
  {
    _str = mov._str;
    mov._str=NULL;
  }
  // Normal constructor
  myString(const char* str)
  {
    size_t len = strlen(str);
    _str=new char[len];
    memcpy(_str,str,sizeof(char)*len);
  }
  // Destructor that deallocates our string
  ~myString()
  {
    if(_str!=NULL) // Make sure we only delete _str if it isn't NULL!
      delete [] _str;
  }
  void Set(myString&& str)
  {
    _set<myString&&>(std::move(str));
  }
  void Set(const myString& str)
  {
    _set<const myString&>(str);
  }


  // Assignment operator, does the same thing the copy constructor does, but also mimics the destructor by deleting _str. NOTE: It is considered bad practice to call the destructor directly. Use a Clear() method or something equivalent instead.
  myString& operator=(const myString& right)
  {
    delete [] _str;
    size_t len = strlen(right._str)+1; //+1 for null terminator
    _str=new char[len];
    memcpy(_str,right._str,sizeof(char)*len);
    return *this;
  }

private:
  template<typename U>
  void _set(U && other)
  {
    _str=std::forward<U>(other);
  }

  char* _str;
};
{{</pre>}}
Notice the use of {{<code>}}std::move(){{</code>}} to transfer the rvalue correctly, followed by {{<code>}}std::forward<U>(){{</code>}} to forward the parameter. By using this, we avoid redundant code, but can still build move-aware data structures that efficiently assign values with relative ease. Now, its on to [Part 5: Delegated Llamas](#)! Or, well, delegates, function pointers, and lambdas. Possibly involving llamas. Maybe.
