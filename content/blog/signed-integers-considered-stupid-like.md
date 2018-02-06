+++
blogimport = true
categories = ["blog"]
comments = [3162371866536579924, 4472881356198965684, 713224613903183883, 4129433169897295656, 5552192573693996322, 8828581802798000851, 3239997872647136827, 3840201350124735426, 7716104787131010417, 3168869092929364582, 2815354709836820985, 3095904484010271963, 8547136227912950141]
date = "2011-11-24T16:02:00Z"
title = "Signed Integers Considered Stupid (Like This Title)"
updated = "2013-05-10T02:01:14.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
*Unrelated note: If you title your article "[x] considered harmful", you are a horrible person with no originality. Stop doing it.*

Signed integers have always bugged me. I've seen quite a bit of signed integer overuse in C#, but it is most egregious when dealing with C/C++ libraries that, for some reason, insist on using {{<code>}}for(int i = 0; i < 5; ++i){{</code>}}. Why would you *ever* write that? {{<code>}}i{{</code>}} cannot possibly be negative and for that matter shouldn't be negative, ever. Use  {{<code>}}for(unsigned int i = 0; i < 5; ++i){{</code>}}, for crying out loud. 

But really, that's not a fair example. You don't really lose anything using an integer for the i value there because its range isn't large enough. The places where this become stupid are things like using an integer for height and width, or returning a signed integer count. Why on earth would you want to return a negative count? If the count fails, return an unsigned -1, which is just the maximum possible value for your chosen unsigned integral type. Of course, *certain people* seem to think this is a bad idea because then you will return the largest positive number possible. What if they interpret that as a valid count and try to allocate 4 gigs of memory? Well gee, I don't know, what happens when you try to allocate -1 bytes of memory? In both cases, something is going to explode, and in both cases, its because the person using your code is an idiot. Neither way is more safe than the other. In fact, signed integers cause far more problems then they solve. 

One of the most painfully obvious issues here is that virtually every single architecture in the world uses the [two's complement](http://en.wikipedia.org/wiki/Two's_complement) representation of signed integers. When you are using two's complement on an 8-bit signed integer type (a {{<code>}}char{{</code>}} in C++), the largest positive value is 127, and the largest negative value is -128. That means a signed integer can represent a negative number so large *it cannot be represented as a positive number*. What happens when you do {{<code>}}(char)abs(-128){{</code>}}? It tries to return 128, which overflows back to... -128. This is the cause of a host of security problems, and what's hilarious is that a lot of people try to use this to fuel their argument that you should use C# or Java or Haskell or some other esoteric language that makes them feel smart. The fact is, any language with fixed size integers has this problem. That means C# has it, Java has it, most languages have it to some degree. This bug doesn't mean you should stop using C++, it means you need to *stop using signed integers in places they don't belong*. Observe the following code:

{{<pre cpp>}}if (*p == '*')
  {
    ++p;
    total_width += abs (va_arg (ap, int));
  }{{</pre>}} 
  
This is *retarded*. Why on earth are you interpreting an argument as a signed integer only to then immediately call {{<code>}}abs(){{</code>}} on it? So a brain damaged programmer can throw in negative values and not blow things up? If it can only possibly be valid when it is a positive number, interpret it as a *{{<code>}}unsigned int{{</code>}}*. Even if someone tries putting in a negative number, they will serve only to make the {{<code>}}total_width{{</code>}} abnormally large, instead of potentially putting in -128, causing {{<code>}}abs(){{</code>}} to return -128 and creating a {{<code>}}total_width{{</code>}} that is far too small, causing a buffer overflow and hacking into your program. And don't go declaring {{<code>}}total_width{{</code>}} as a signed integer either, because that's just stupid. Using an unsigned integer here closes a potential security hole and makes it even harder for a dumb programmer to screw things up{{<sup>}}<a href="#foot1">1</a>{{</sup>}}.  

I can only attribute the vast overuse of {{<code>}}int{{</code>}} to programmer laziness. {{<code>}}unsigned int{{</code>}} is just too long to write. Of course, that's what {{<code>}}typedef{{</code>}}'s are for, so that isn't an excuse, so maybe they're worried a programmer won't understand how to put a -1 into an {{<code>}}unsigned int{{</code>}}? Even if they didn't, you could still cast the {{<code>}}int{{</code>}} to an {{<code>}}unsigned int{{</code>}} to serve the same purpose and close the security hole. I am simply at a loss as to why I see {{<code>}}int{{</code>}}'s all over code that could never possibly be negative. If it could never possibly be negative, you are therefore *assuming* that it won't be negative, so it's a much better idea to just make it *impossible* for it to be negative instead of giving hackers 200 possible ways to break your program. 


{{<span style="font-size:80%">}}<sup><a name="foot1">1</a></sup> There's actually another error here in that {{<code>}}total_width{{</code>}} can overflow even when unsigned, and there is no check for that, but that's beyond the scope of this article.{{</span>}}
