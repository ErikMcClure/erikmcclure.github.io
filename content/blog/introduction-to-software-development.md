+++
categories = ["blog"]
date = "2020-04-04T04:33:00Z"
draft = true
title = "Introduction To Software Development With C#"
+++
This is intended as a guide for absolute beginners, who have no idea how computers work but want to learn a programming language. For demonstration purposes, I will be using C#, but the concepts apply to any language. This tutorial is not intended as a quick way to learn C# - it is intended for those who want (or need) to *understand* how a computer works and how to convince it to do things. We're going to start with the basics, and you may already know some of them, but **I strongly encourage you to read through everything anyway**, because there is a big difference between *knowing* how a computer works and *understanding* how it works. To assist those of you with short attention spans, **I will be highlighting important parts in bold**. Even if you try to skim this section, at least pay attention to the **bold text**.

## The Prologue
Before we begin talking about software, we need to talk about hardware. Specifically, we need to be aware of how the computer works at a basic level, because this affects how we design our programs. At this point, most people know how a computer works, or at laest think they do, and the components I'll be talking about will be familiar to anyone whose ever built a PC. I will be highlighting some very important subtle details in bold that you might not be aware of, even if you've previously built your own computer.

{{<img src="/img/intro0.png" alt="Computer Hardware Structure" width="710">}}

### RAM
RAM (**R**andom **A**ccess **M**emory) is your computer's memory. It stores all the data that is currently being operated on. This fact is a bit hard to really appreciate, so let me emphasize it: **Everything that is happening on your computer is in your RAM**. That includes all the data, images, and even the *code* that is displaying those images. **All the programs actively running on your computer? They have to be in RAM too.** Code is not some imaginary thing that exists outside of memory, it has to be loaded into memory to be executed. Every single aspect of your computer, the OS, the programs, the images, everything is in RAM. **If it's not in RAM, the CPU can't run instructions on it.**

Your computer's entire state is the RAM. Every time the CPU clock ticks, the CPU reads some numbers from RAM and writes some new numbers out, a few bytes at a time. Only by doing this *billions of times a second* do we get the illusion of images smoothly moving across a monitor. As a programmer, you are the one telling the CPU which numbers to read and write. In order to achieve something, your program will need to do this billions of times a second. Luckily, we have a bunch of libraries to make all this much easier, but always remember what's going on underneath the hood - a bunch of numbers changing billions of times a second.

### CPU
The CPU (**C**entral **P**rocessing **U**nit) *decides* what to do with the numbers in your RAM, or it *loads* numbers into RAM from somewhere else. That's the only thing it can do. Every CPU cycle it needs to decide what it's going to do next. So, it looks at a particular spot in RAM for a number. That number corresponds to some kind of *instruction* for the CPU to execute. Instructions are simply things the CPU can do - add numbers, subtract numbers, read a number from RAM, write a number to RAM, etc. The number it read from RAM tells it to, for example, add two numbers together from two particular parts of RAM. So, it then fetches the two numbers from wherever they are in RAM, adds them together, and then writes the result somewhere else to RAM. If it does this a billion times a second you get a video playing on your screen.

The important thing to keep in mind here is that the CPU instructions can only directly access RAM. It can't read two numbers off of your hard drive to add them together. This means we have to spend time telling the CPU to first **load the numbers into RAM** in order to work with them. The CPU is actually way more complex than this and has a bunch of cache layers and pipelining nonsense, but none of that is important so don't worry about it. As far as we're concerned, **CPUs read numbers from RAM, do a thing, then write the result back into RAM.**

### GPU
The GPU (**G**raphics **P**rocessing **U**nit) is a special kind of chip that's really fast at doing graphics related calculations. Just like the CPU, it has it's own RAM, called V-RAM (**V**ideo RAM), and **it can't touch the CPU's RAM**. Consequently, if the CPU wants to get the GPU to do anything, it has to transfer bytes from RAM into V-RAM, give the GPU a list of instructions, and then wait for the GPU to say "okay I'm done" before it can access the results. We're skipping over a huge amount of complexity, but the important takeaway here is that **GPUs require special code to run**, and as a result we can't simply write code that magically works on a GPU. We need to write special code that the CPU then *loads into V-RAM* so the GPU can access it, which requires some sort of API like DirectX or OpenGL. This complexity is why **we won't be doing any graphics related stuff in this tutorial** unless we're using an external library that can do all this complex work for us.

### Storage
Hard drives or SSDs or NVMe drives or whatever you have in your computer are bulk, permanent storage devices. They can store lots and lots of information, but our CPU can't execute instructions on them. Instead, **the CPU needs to load data from our bulk storage device into RAM** before we can do anything. This is why programs have to "open" files - they are often reading the entire file into RAM so that the CPU can run instructions on the contents.

Of course, some files are so large they won't fit into RAM. Programs deal with this by only reading *part* of a file into RAM, the part that we're actually executing instructions on. We don't need to worry about this, as all our files are small.

## The Setup
Before we actually get started writing code, you need to download an IDE (**I**ntegrated **D**evelopment **E**nvironment), which contains a C# Compiler. The *compiler* is what takes our human-readable program and turns it into a bunch of numbers that the CPU understands. This is a fairly complex multi-stage process, but the IDE will handle everything for us so we don't need to worry about it.

I recommend downloading the [free, community version of Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16). Check this when installing it - you can install other components if they interest you, but we won't be using them.

{{<img src="/img/intro1.png" alt="Visual Studio Setup" width="710">}}

Once the program starts up, click on "Create a New Project"

{{<img src="/img/intro2.png" alt="Creating A New Project" width="710">}}

Type "console" in the search bar and select "Console App (.NET Framework)" - make sure it says `C#`.

{{<img src="/img/intro3.png" alt="Finding the project type" width="710">}}

Name it whatever you want and put it in a location you can find again later. You can put the solution and project folders in the same directory if you want - it won't matter. If you're wondering what a "Solution" is, it's just a collection of projects, and each project corresponds to a single EXE. We're only making a single executable program right now, so it doesn't matter what you do with the solution.

### The Windows

Your new, empty program will now show up in a new window of Visual Studio. Visual Studio is huge and complex, but you don't need to worry about what all the buttons do. All we care about right now are two windows, the text editor and the Solution View.

{{<img src="/img/intro4.png" alt="Visual Studio Text Editor" width="718">}}

This is the core window we care about. Up at top is a list of tabs of open files - right now we only have one file, `Program.cs`, and we're currently looking at it. Along the left-hand side you'll see some numbers - those are just line numbers, which is useful when you're trying to find a specific line in a file filled with thousands of lines of source code. The dropdown boxes up at top are simply more ways to jump to different parts of the file, again to help with huge files filled with thousands of lines of code. The little `[-]` boxes next to the text allow you to collapse chunks of the source code, to make it easier to deal with - you guessed it - huge files filled with thousands of lines of code! Gee, I wonder what professional programmers spend all day doing? Luckily, we won't have to worry about that, because all our programs are going to be nice and {{<span style="font-size:80%;letter-spacing:2px;">}}small{{</span>}}.

At the bottom are some statistics about the file, like what line you're on, the column you're on, the current zoom level (which should be at 100%), what the line endings are, and some other information that isn't really that important right now. What is important is that there's many different colors of text. This is called **Syntax Highlighting**, and it's used to make it easier to visually parse what's going on in a program. Unfortunately, we have no idea what's going on, so it's not particularly helpful right now. Instead, let's look at the other window we care about

{{<img src="/img/intro5.png" alt="Visual Studio Project Explorer" width="368">}}

This is a list of our projects, and all the files contained in those projects. Of course, we only have one project right now, with one file, so it's pretty empty. However, once you have a project that's complex enough to have multiple files, you will want to refer back to this window. In addition to these windows, there's also a toolbar containing your usual Save/Save As/Open/etc. buttons, along with a bunch of other buttons that we don't really care about right now. We do care about the little green "Start" button, however:

{{<img src="/img/intro6.png" alt="Visual Studio Toolbar" width="730">}}

Go ahead, click it! Visual Studio will say something about "compiling", and then a console window will appear for a brief moment before... vanishing. Right, our program doesn't actually do anything. Let's fix that!

### Basic Syntax

{{<pre csharp>}}using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
{{</pre>}}

This is the empty program that Visual Studio has created for us. We can't really discuss how everything here *actually* works without going through the rest of the tutorial, but it's important to have a general idea of what all the funny characters mean, so we'll go through a simplified overview of the syntax. The syntax overview is optional, so if you think you know what's going on, feel free to skim over it - I've highlighted the more important bits in **bold**. If you get confused, you can always come back to this section! Now, lets first discuss what a `namespace` is.

### Namespaces
Most of this empty program is actually just boilerplate code managing a bunch of namespaces. What is a `namespace`? A namespace is just a way to **organize code**. For example, the C# standard library has *millions* of things inside of it. If we just listed them all in alphabetical order it would be a nightmare to find anything! Instead of doing that, we use namespaces to *group similar code together*. Like categories of books in a library, or genres of music, it's just a handy way of categorizing things. The first namespace referenced here is `System`, which is just the standard library. The C# standard library contains a whole bunch of helper code designed to make common tasks easier for you, like opening a file or writing text, and by grouping all these helpers in a namespace called `System`, it makes it easier to keep our own code seperate from the helper code. In fact, we've already declared our own namespace, called `ConsoleApp1`, which is just the name of our project.

Now, you may have guessed from seeing a statement like `System.Collections.Generic` that you can *nest namespaces*. This means that a namespace can contain another namespace. This is the same as a folder in Windows containing other folders, or genres of music containing more fine-grained subgenres.

#### Names and Dots
When we have nested namespaces, we need a way to access the things inside of it. C# uses a period `.` for this purpose. If you type `System.` on a blank line somewhere in our file, the IDE will actually give you a list of everything contained inside of System! And if you type `System.Text.`, it will show you everything that's inside the `System.Text` namespace, and so on. `System.Text.Decoder` simply means "I want to access the Decoder object that's inside the Text namespace that's inside the System namespace". This is similar to how file paths work in Windows, except instead of typing `C:\Windows\System32`, you would express it as `C.Windows.System32`.

The catch is that, unlike folders, **names cannot have spaces**. I don't just mean names of namespaces, I mean ***all the names***. Namespaces, classes, objects, functions, types, whatever, **all names must start with a letter and cannot contain any spaces**. They can have numbers on the end, or in the middle, and they can use an underscore `_` instead of a space, but that's it. The reason for this is because, if we allowed spaces or names that started with a number, a bunch of things become ambiguous and impossible to resolve. This naming restriction is almost universal across all programming languages, but there are some notable exceptions, so always double-check the syntax for the language you're using!

#### Classes
Inside our `ConsoleApp1` namespace (or whatever you called your project), we also declare a class called `Program`. Classes are more complicated than namespaces, but we don't need to worry about that right now. All this particular class is doing is the same thing a namespace does - it simply contains `Main`, which is our entry-point. That's it. We can access Main by typing `ConsoleApp1.Program.Main` if we wanted to. It's just another way of organizing code.

### Statements and Semicolons
Okay, so we understand the *concept* of a namespace, but we haven't looked at the actual syntax here yet. Let's pick apart the first line:

{{<pre csharp>}}using System;{{</pre>}}

This is called a **statement**. A statement is a single *action* that the code should do, and is often just a single line of code. What a statement does depends on where in the file it is. This statement is at the top of the file, and it starts with the `using` keyword. You'll notice that `using` is also highlighted in blue, which is how we know it's a **keyword**, a special word that tells the compiler to do something in particular. `using` tells the compiler to use a particular namespace. The next word that comes after `using` is the namespace we want to use.

This is followed by a semicolon (`;`), and yes, that semicolon is very important. It marks the **end of the statement**. Pressing enter and starting a new line **does not end a statement!** Only a semicolon does. This is not true for all programming languages, however, so it's always important when learning a new language to familiarize yourself with its particular syntactic ideosyncracies. C# requires semicolons at the end of every statement, and statements can be spread out over multiple lines. 

#### Using
So what does it mean to "use" a namespace? Well, remember that all our helper code is organized into namespaces, but that means that if we want to *use* any of it, we have to write this over and over:

{{<pre csharp>}}System.Text.UTF8Encoding.Convert("something");
System.Text.UTF8Encoding.Convert("something else");{{</pre>}}

That gets really obnoxious really fast, but if we put `using System;` at the start of our file, we can instead write

{{<pre csharp>}}Text.UTF8Encoding.Convert("something");
Text.UTF8Encoding.Convert("something else");{{</pre>}}

This is better, but can we eliminate even more? `Text` is just a namespace too, so lets "use" that one with the statement `using System.Text;`. Now we can write:

{{<pre csharp>}}UTF8Encoding.Convert("something");
UTF8Encoding.Convert("something else");{{</pre>}}

This is much more convenient, but of course, if we had something called `UTF8Encoding`, it'd be ambiguous! As a result, we should be careful about which namespaces we use to avoid accidentally causing naming conflicts.

### Declarations
Okay, we know what the `using` statements are, so what's the deal with `namespace ConsoleApp1`? It doesn't have a semicolon on the end! This is a **namespace declaration**, followed by a **block**. `namespace` is a keyword that tells the compiler to create a new namespace with whatever name comes after `namespace`. So `namespace ConsoleApp1` creates a new namespace called `ConsoleApp1`. This is followed by a left curly brace `{`, which begins the *block*. We write whatever things we want contained inside our namespace, and then we close the block with a right curly brace `}`.

Why are we using curly braces here instead of more semicolons? The reason is that **we can nest curly braces**. You can't nest semicolons, because the next statement simply starts after the last semicolon. By using curly braces for blocks, we can nest them, and in fact block can contain many other blocks:

{{<pre csharp>}}{
  {
    // ...
  }
  {
    {
      // ...
    }
    {
      // ...
    }
  }
}
{{</pre>}}

The exact meaning of a given block depends on where it's being used. When it's right after the `namespace` keyword, that means everything inside the block belongs inside the namespace. The idea of "blocks" is used in many programming languages, but not all of them use curly braces to denote the beginning and end of a block - check the syntax of whatever language you're using to figure out how it defines "blocks".

#### Scoping
Inside our namespace block, we find the statement `class Program`. This means we are telling the compiler to create a new `class` called `Program`. Right now, we're just using this class to organize things. Because we're declaring this new class *inside a block*, it is now **scoped** to that block. "Scoping" something to a block simply means that it is contained inside that block. In this case, our class is contained inside our namespace `ConsoleApp1` and so it is only accessible from inside the namespace, or by using `ConsoleApp1.Program`. Right now, the only thing we're actually putting inside the class is `static void Main`, which is the entry-point to our program.

### The Entry Point
When we tell the operating system to run our program, how does it know what to do? Lets go back to our concept of everything just being a bunch of numbers. When we "compile" our program, we turn our human-readable textual representation into a bunch of numbers the CPU can understand. However, only some of these numbers are actually instructions - others are data, or pictures, or text, so we need to find where in the program the first "instruction" number is. The operating system does this by storing all executables in a [common format](https://en.wikipedia.org/wiki/Portable_Executable). This format organizes all the code, data, and other information in a neat little package that's easy to read. This lets the compiler include an "entry point" alongside our code. This "entry point" is just another number referring to a specific location inside our program. The operating system reads this number, jumps to that part of our executable file, then tells the CPU to execute whatever number is there.

Okay, that's how the compiler tells the *operating system* where to start running our program, how do we tell the *compiler* where to start executing? This depends on what programming language you use. We're using C#, and C# looks for something called `Main`. `Main` can be in any class anywhere in the program, so long as it exists somewhere, but **there can only be one**. If you have multiple things called `Main`, the compiler gets confused and you have to fiddle with some project settings to tell it which `Main` it should actually use.

Now, if `Main` is our entry point, that means we should write our first program inside of it. Don't worry about the specific syntax of `static void Main(string[] args)`, we don't need to worry about it right now. All we need is the empty block that starts right afterwards - this is where we're going to write our code.

## Our First Program
Our first program is going to be very simple: we will ask the user to type in a number, then we will ask the user to type in a second number. We will add the two numbers together and display the result. I'm going to explain how to write this line-by-line, but if you're impatient, here is the full program we'll be writing out. You can click on any line and it will jump to an explanation of it.

{{<pre csharp>}}using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            string input1;
            Console.Out.Write("Type the first number and press enter: ");
            input1 = Console.In.ReadLine();
            int number1 = Convert.ToInt32(input1);
            Console.Out.Write("Type the second number and press enter: ");
            int number2 = Convert.ToInt32(Console.In.ReadLine());

            int result = number1 + number2;
            Console.Out.Write("Your number is ");
            Console.Out.WriteLine(Convert.ToString(result));
            Console.Out.WriteLine("\nPress any key to exit!");
            Console.ReadKey();
        }
    }
}
{{</pre>}}

### Variables and Types
{{<pre csharp>}}string input1;{{</pre>}}

In this statement, we are declaring a new variable called `input1`. What is a variable? A variable is simply a place to store things. Of course, everything on a computer is just a bunch of numbers, so we're really just telling the compiler "I need a box to put numbers in". `string` is the **type** of the variable, and it tells the compiler how big our box needs to be. So `string input1` tells the compiler that we need a box called `input1` that's big enough to hold a `string`, and then we end our statement with a semicolon `;`.

If `string` is a **type**, what exactly is a **type**? Well, not all numbers are treated like the kind of numbers you can add together. Some numbers will be used as text. Some numbers are used as images. While everything we do with computers might be numbers under the hood, how we *interpret* those numbers changes what they represent. A "type" is just what it sounds like - it's a type of number. Lets look at a few simple examples:

- **Boolean**: A boolean number (represented as `bool` in C#) is one that can only be `True` or `False`. Basically, it means the number will only ever be `0` (`False`) or `1` (`True`). The compiler won't let you add booleans together, because that doesn't make any sense. They can only be `True` or `False`, and you can't add `True` to `True`. There's no such thing as `Extra True`. How we interpret data determines the kinds of operations we can do with that data. In this way, **types protect you from accidents** by making it impossible to do something that would result in a nonsensical value. 
- **Integer**: An integer is just a whole number, and behaves the same way that you would expect, but it has a **limited range**. C#'s default integer type (`int`) can represent any value between $$-2^31$$ (-2,147,483,648) and $$2^31 - 1$$ (2,147,483,647). Integers can only store **whole numbers**, they can't store fractions, and division will truncate the decimal part. For example, `8 / 5` is equal to `1`, even though the true result is `1.6`, because the integer throws away everything past the decimal point instead of rounding. This can result in some very unintuitive behavior when dividing integers that [we will go over later](#Numbers).
- **Float**: A floating point number (represented as `float` in C#) can represent very large numbers and fractions, although not exactly. `8.0 / 5.0` evaluates to `1.6`, but `1.0 / 3.0` evaluates to `1.333334`, because it can't be exactly represented. By default, the compiler assumes any number without a decimal point is an integer (like `1`), and any number with a decimal point is a floating point number (like `1.0`). I'll go over exactly how floating point numbers work [later](#Numbers).
- **Character**: A character (represented as `char` in C#) represents a single ASCII letter. What's ASCII? ASCII is just an agreement on what numbers correspond to what letters. The exact values aren't important, but if you're curious, [you can look them up here](http://www.asciitable.com/). This standard is how we convert a bunch of numbers into words displayed on your screen. If the computer encounters the number `73`, for example, it will print `I`. For non-english characters, there's a much more complex standard called Unicode, but we're only dealing with english right now. Unlike numbers or true/false values, you can't just type `Z` and have a character come out, because the compiler might mistake it for a variable called `Z`. If we want a character value, we need to surround it in single-quotes, like this: `'Z'`. That will evaluate to a value of type `char`.

These are known as *primitive types*, because they cannot be broken up any further - they are single entities. In addition to these primitive types, there are also *aggregate types*, which are made up of primitive types. Let's start by looking at an Array.

- **Array**: An array is a *list* of a specific type that's a certain length. For example, we might have an array of 4 integers, or an array of 16 floats. Sometimes, we don't know the length of the array until we run the program. We represent arrays in the program syntax by appending `[]` to whatever type the array is. For example, an array of integers would be `int[]`, and an array of floating point numbers would be `float[]`. In fact, you can have arrays of arrays, too, like `int[][]`, which is an array whose type is an array whose type is an integer.

42 | Integer (`int`)
3.14159 | Float (`float`)
true | Boolean (`bool`)
{ 1, 2, 3 } | Array of Integers (`int[]`)
'Z' | Character (`char`)
{ 'H', 'e', 'l', 'l', 'o', '!' } | Array of Characters (`char[]`)
"Hello!" | String (`string`)

We can see here that a `string` is really just a special kind of **charactar array**. Under the hood, a string and a character array are the same, but a string has special properties that make it easier to work with, which is why we can't assign `char[]` to `string`. Despite this, a string behaves just like `char[]` and you can iterate through each of it's characters just as you would a normal array.


Structure - A structure is a *collection* of one or more *named elements*, each of which can be a different type. It's a bit hard to *describe* these, so instead let's look at some examples:

Or an Array of an Array of an Array. Things can get complicated pretty fast, so we'll keep our types simple for this tutorial, but it's important to realize that **all types are either primitive types, arrays of types, or structures containing types**. No matter how complex a type is, fundamentally it's just a bunch of integers, floats, and characters. This applies to *any type system* in *any programming language* - all additional types are just extensions of these basic concepts.

So, using our type system, we can tell the compiler what we are using a number to represent. Sometimes it's a number is just a number, but sometimes it's a letter, or something else entirely. 

### Calling Functions


### Assignment

Now, we haven't actually put anything inside the box yet. Does that mean the box is empty? In some languages, the compiler will clean out the box before it gives it to you. However, remember that your computer's RAM is just a giant bunch of numbers. There's only so much memory to go around, which means we probably got someone else's old memory, and our box is full of junk. Because C# does not initialize variables for us, our box is currently full of random junk, and C# won't let us use it. C# requires that we *put something in the box first*, which means we need to **assign a value to the variable**.

### String Operations
In order to display a number, we have to convert it into a string. We can't directly display a number because computers store numbers in binary, not human-readable base 10, so we have to convert it into our familiar decimal representation.


### Returning


## Debugging







It's important to remember that your computer is deterministic - if you start running the computer from a given state, it will *always* give you the same result. This often doesn't happen because the "state" of your program isn't just the values your program has - it's the state of the entire operating system and the machine it's running on. Your "state" is simply whatever numbers have been loaded into RAM, and if you're doing things over the internet, it depends on whatever numbers you recieve back. Once the "state" your deterministic program depends on includes *the entire internet*, it's easy to see why modern programs don't seem very deterministic at all. Program are still deterministic, but the potential state that could influence them is now so enormous and so complex it's basically random.

With that said, good software tries to **minimize the amount of state you have to worry about**. The function that adds two numbers together should only ever be affected by those two numbers. Try to keep all the code that talks to the OS in one place. Try to keep all the code that talks to a website in one place. This way, when something goes wrong, the amount of things that could have caused a function to go wrong are drastically reduced, which makes debugging easier.



## If Statements
Lets modify our program so that it can either add two numbers together, or add three numbers. We'll do this by adding a third prompt, then checking to see if the user actually entered anything into the console before pressing enter. If they entered a third number, add it to the first two. If they didn't enter a third number, just add the first two numbers and exit.


## Functions
We can make our program more general by allowing the user to skip any of the three numbers. We'll do this by taking advantage of a property of addition: If you add 0 to something, the result doesn't change. So, all we have to do is detect if the user didn't enter a number, and treat it as 0.





This works, but there's a lot of copy+pasted code in that. As a programmer, you should avoid repeating yourself. This is both to save time, and to minimize bugs. In fact, there seems to be a bug in the above code! Can you find it?

[...]

When I was copy and pasting those lines, I forgot to change one of the variable names from `num2` to `num3`. Given that these lines are so similar, with only the variables changing, is there a way to express the same logic that's less error prone?

The answer we're looking for is a Function.





## References

What if I told you there was another way to write our function? What if it never returned anything?



## Loops

Lets go back to our simple example of adding three numbers together. What if we wanted to add any amount of numbers together? As many numbers as you pass into the program? While we factored out the input check, we can't just call our function 2147483648 times. Instead, we're going to use a **loop** to accomplish this for us.

### While Loop
There are several kinds of loops, but we're going to start with the simplest, a **while loop**.

[]


### For Loop

A for loop is simply a more compact way of representing a loop that contains a counter. With our while loop, we had to declare a variable called "counter" to store what iteration we were on, then increment it every loop. The for loop does this for us:



The three sections of the for loop don't have to be there, and can actually execute any code we want. I can remove each part and it will still work:




Yes, `for(;;)` is a valid loop, and it simply runs forever until you break out of it.

### Do While

This is simply a slight modification of our while loop. The catch is that the condition is evaluated at the end of the loop, which means this code will *always* run at least once. This can occasionally be useful.

### Foreach Loop


## Recursion

There is another way to represent a loop, but it can seem a bit unintuitive at first.

To begin, we're going to start with our ridiculous looking `for(;;)` loop, where we've done everything explicitely.



Next, we're going to move the entire body of the loop into a function that takes our counter as a parameter




This, of course, doesn't quite work - if we run this, we will simply call the function with `0` over and over again, and it won't even compile because `break` doesn't have anything to break out of. It only knows about the function it's inside of, and there's no loop inside of it.

Okay, but what if we called





## Numbers

Fundamentally, a computer is a bunch of numbers. **Everything is a number** once you get close enough to the hardware. The CPU instructions are numbers. Images are numbers. Text is numbers. The hardware values controlling how bright your monitor is are just numbers. This fact is easy to think about, but hard to appreciate. 




A floating point number can be thought of as a set of digits that be offset anywhere you want:

12345678.0
0.00000012345678
12345678000000000000000000.0

We can't change any of the zeroes outside the precision range, we can only move the decimal point around. This is why it's called "floating-point" - the decimal point "floats" around.






## Algorithmic Complexity

What is an "Algorithm"? An algorithm is just a function - you pass parameters into it and get a result out. That's it. Why do mathematicians insist on using algorithm instead of function? Bbecause it sounds cool. Before we talk about data structures, we need to talk about complexity. Normally, this is a very math-heavy introduction to how you calculate the Big-O property of algorithms, but almost all of this is ignored in the real world anyway, so instead we're going to focus on how to use Big-O to **estimate costs**.

Essentially, whenever we talk about something we're doing to `N` things, we want to know how long it's going to take relative to how many things there are. You might be thinking that it's just going to take twice as long for twice as many things, but this isn't always true. When an operation takes two times as long with 2 things, and three times as long with 3 things, etc., we call this **linear time**, and it's represented with `O(n)`. Let's look at an example:

// add n numbers together

Given `N` numbers, we can assume adding a number to another number is, say, 1 CPU cycle. So that means this will take `N` CPU cycles. 

Okay, so how would we get into a situation where it wasn't just `N` cycles? Lets say that we're given `N` numbers again, but this time we're trying to see if the same number ever appears more than once. The simplest way to do this is to go through each number, then compare it against every other number.

// compare n numbers with nested for loop

Here we have the dreaded **nested for loop**. Nested for loops are scary because they have exponential blowup. We're executing `N` comparisons `N` times, which is `N * N` comparisons, or just `N²`. That means the complexity of this function is O(N²), because it grows exponentially. If we had three for loops nested inside each other, it'd be O(n^3), four loops would be O(n^4), and so on. Even with how powerful our computers are today, exponential blowup will quickly overwhelm them. Try calling this function with 1000 and it'll take a few seconds to complete even on a modern CPU, and that's just with one thousand numbers! There are people on twitter with more followers than that! As a result, we generally want to avoid O(n²) and prefer algorithms that run faster, like in linear time. There is a way to change that comparison function to be linear time, but it requires a data structure i'll be introducing later. For now, let's consider this:

// Choose a random number out of n numbers by repeatedly checking a random number

Given `N` numbers, this picks one at random and returns it in O(n) time (linear time). However, we can do much better. Instead of picking our way through the array, why not just go straight to the element we want? We know how big the array is, after all, so let's just generate a random number between 0 and `N`

// Choose a random number properly

No matter how many numbers we pass in, this always takes the same amount of time. This is represented as `O(1)` for **constant time**, which is considered the best algorithmic complexity, because you can put in 5 billion numbers will take the same amount of time as 12. Here's a handy table:

O(N!) factorial time Very Bad
O(N^2) exponential time Bad
O(log(N)*N) sub exponential time Good
O(N) Linear time Very good
O(log(N)) Sublinear time Excellent
O(1) Constant time Fantastic

But wait, what's log(n)? That's just a fancy math way of saying the inverse of an exponential. So, if you put in log(4), you get 2, because 2*2 is 4. If you put in log(16), you get 4, because 4*4 is 16, and so on. It's called a base-2 logarithm, if you really want to look it up, but knowing the math behind it isn't really necessary. To understand why it behaves that way, you just need to understand Data Structures.

## Data Structures


### Array


### Linked List

#### Stack

#### Queue

### Hash

### Binary Tree

#### Sorted Binary Tree

#### Heap

### Graph
A graph is not actually a data structure by itself, but because so many problems can be reduced to graph theory it's critically important to know what it is and how it works.

## Memoization


## Conclusion





