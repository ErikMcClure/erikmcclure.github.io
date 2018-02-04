+++
blogimport = true
categories = ["blog"]
date = "2017-02-13T21:33:00Z"
title = "Windows Won't Let My Program Crash"
updated = "2017-02-14T03:31:45.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
It's been known for a while that windows has a bad habit of [eating your exceptions](http://blog.paulbetts.org/index.php/2010/07/20/the-case-of-the-disappearing-onload-exception-user-mode-callback-exceptions-in-x64/) if you're inside a WinProc callback function. This behavior can cause all sorts of mayhem, like your program just vanishing into thin air without any error messages due to a stack overflow that terminated the program without actually throwing an exception. What I didn't realize is that it also eats {{<code>}}assert(){{</code>}}, which makes debugging hell, because the assertion would throw, the entire user callback would immediately terminate without any stack unwinding, and then windows would just... keep going, even though the program is now in a laughably corrupt state, because only half the function executed.

While trying to find a way to fix this, I discovered that there are no less than 4 different ways windows can choose to eat exceptions from your program. I had already told the kernel to stop eating my exceptions using the following code:
{{<pre>}}HMODULE kernel32 = LoadLibraryA("kernel32.dll");
  assert(kernel32 != 0);
  tGetPolicy pGetPolicy = (tGetPolicy)GetProcAddress(kernel32, "GetProcessUserModeExceptionPolicy");
  tSetPolicy pSetPolicy = (tSetPolicy)GetProcAddress(kernel32, "SetProcessUserModeExceptionPolicy");
  if(pGetPolicy && pSetPolicy && pGetPolicy(&dwFlags))
    pSetPolicy(dwFlags & ~EXCEPTION_SWALLOWING); // Turn off the filter 
{{</pre>}}However, despite this, COM itself was wrapping an entire {{<code>}}try {} catch {}{{</code>}} statement around my program, so I had to figure out how to turn that off, too. Apparently [some genius at Microsoft](https://blogs.msdn.microsoft.com/oldnewthing/20110120-00/?p=11713/) decided the default behavior should be to just swallow exceptions whenever they were making COM, and now they can't change this default behavior because it'd break all the applications that now depend on COM eating their exceptions to run properly! So, I turned that off with this code:
{{<pre>}}CoInitialize(NULL); // do this first
if(SUCCEEDED(CoInitializeSecurity(NULL, -1, NULL, NULL, RPC_C_AUTHN_LEVEL_PKT_PRIVACY,
  RPC_C_IMP_LEVEL_IMPERSONATE, NULL, EOAC_DYNAMIC_CLOAKING, NULL)))
{
  IGlobalOptions *pGlobalOptions;
  hr = CoCreateInstance(CLSID_GlobalOptions, NULL, CLSCTX_INPROC_SERVER, IID_PPV_ARGS(&pGlobalOptions));
  if(SUCCEEDED(hr))
  {
    hr = pGlobalOptions->Set(COMGLB_EXCEPTION_HANDLING, COMGLB_EXCEPTION_DONOT_HANDLE);
    pGlobalOptions->Release();
  }
}
{{</pre>}}There are two additional functions that could be swallowing exceptions in your program: {{<code>}}_CrtSetReportHook2{{</code>}} and {{<code>}}SetUnhandledExceptionFilter{{</code>}}, but both of these are for SEH or C++ exceptions, and I was throwing an assertion, not an exception. I was actually able to verify, by replacing the assertion {{<code>}}#define{{</code>}} with my own version, that throwing an actual C++ exception *did* crash the program... but an assertion didn't. Specifically, an assertion calls {{<code>}}abort(){{</code>}}, which raises {{<code>}}SIGABRT{{</code>}}, which crashes any normal program. However, it turns out that Windows was eating the abort signal, along with every other signal I attempted to raise, which is a problem, because half the library is written in C, and C obviously can't raise C++ exceptions. The assertion failure even showed up in the output... but didn't crash the program!
{{<pre>}}Assertion failed!

Program: ...udio 2015\Projects\feathergui\bin\fgDirect2D_d.dll
File: fgEffectBase.cpp
Line: 20

Expression: sizeof(_constants) == sizeof(float)*(4*4 + 2)
{{</pre>}}No matter what I do, Windows refuses to let the assertion failure crash the program, or even trigger a breakpoint in the debugger. In fact, calling the {{<code>}}__debugbreak(){{</code>}} intrinsic, which outputs an {{<code>}}int 3{{</code>}} CPU instruction, was *completely ignored*, as if it simply didn't exist. The only reliable way to actually crash the program without using C++ exceptions was to do something like divide by 0, or attempt to write to a null pointer, which triggers a segfault.

Any good developer should be using assertions to verify their assumptions, so having assertions silently fail and then *corrupt the program* is even worse than ignoring they exist! Now you could have an assertion in your code that's firing, terminating that callback, leaving your program in a broken state, and then the next message that's processed blows up for strange and bizarre reasons that make no sense because they're impossible.

I have a hard enough time getting my programs to *work*, I didn't think it'd be this hard to make them *crash*.
