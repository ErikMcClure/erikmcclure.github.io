+++
title = "DirectX Is Terrifying"
date = 2017-02-01T19:35:00Z
updated = 2017-04-09T16:44:26Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

About two months ago, I got a new laptop and proceeded to load all my projects on it. Despite compiling everything fine, my graphics engine that used DirectX mysteriously crashed upon running. I immediately suspected either a configuration issue or a driver issue, but this seemed weird because my laptop had a *newer* graphics card than my desktop. Why was it crashing on *newer* hardware? Things got even more bizarre once I narrowed down the issue - it was in my shader assignment code, which hadn't been touched in almost 2 years. While I initially suspected a shader compilation issue, there was no such error in the logs. All the shaders compiled fine, and then... didn't work.

Now, if this error had also been happening on my desktop, I would have immediately started digging through my constant assignments, followed by the vertex buffers assigned to the shader, but again, all of this ran perfectly fine on my desktop. I was completely baffled as to why things weren't working properly. I had eliminated all possible errors I could think of that would have resulted from moving the project from my desktop to my laptop: none of the media files were missing, all the shaders compiled, all the relative paths were correct, I was using the exact same compiler as before with all the appropriate updates. I even updated drivers on both computers, but it stubbornly refused to work on the laptop while running fine on the desktop.

Then I found something that nearly made me shit my pants.
{{<pre cpp>}}if(profile <= VERTEX_SHADER_5_0 && _lastVS != shader) {
  //...
} else if(profile <= PIXEL_SHADER_5_0 && _lastPS != shader) { 
  //...
} else if(profile <= GEOMETRY_SHADER_5_0 && _lastGS != shader) {
  //...
}{{</pre>}}Like any sane graphics engine, I do some very simple caching by keeping track of the last shader I assigned and only setting the shader if it had actually changed. These if statements, however, have a very stupid but subtle bug that took me quite a while to catch. They're a standard range exclusion chain that figures out what type of shader a given shader version is. If it's less than say, 5, it's a vertex shader. Otherwise, if it's less than 10, that this means it's in the range 5-10 and is a pixel shader. Otherwise, if it's less than 15, it must be in the range 10-15, ad infinitum. The idea is that you don't need to check if the value is greater than 5 because the failure of the previous statement already implies that. However, adding that cache check on the end breaks all of this, because now you could be in the range 0-5, but the cache check could fail, throwing you down to the next statement checking to see if you're below 10. Because you're in the range 0-5, you're of course below 10, and the cache check will ALWAYS succeed, because no vertex shader would ever be in the pixel shader cache! **All my vertex shaders were being sent in to directX as pixel shaders after their initial assignment!**

For almost 2 years, I had been feeding DirectX *total bullshit*, and had even tested it on multiple other computers, and it had never given me a single warning, error, crash, or any indication whatsoever that my code was **completely fucking broken**, in either debug mode or release mode. Somehow, deep in the depths of nVidia's insane DirectX driver, it had managed to detect that I had just tried to assign a vertex shader to a pixel shader, and either ignored it completely, or silently fixed my catastrophic fuckup. However, my laptop had the *mobile* drivers, which for some reason did not include this failsafe, and so it actually crashed like it was supposed to.

While this was an *incredibly* stupid bug that I must have written while sleep deprived or drunk (which is impressive, because I don't actually drink), it was simply impossible for me to catch because it produced *zero errors or warnings*. As a result, this bug has the honor of being both the dumbest and the longest-living bug of mine, *ever*. I've checked every location I know of for any indication that anything was wrong, including hooking into the debug log of directX and dumping all it's messages. Nothing. Nada. Zilch. Zero.

I've heard stories about the insane bullshit nVidia's drivers do, but this is **fucking terrifying**.

Alas, there is more. I had been experimenting with direct2D as an alternative because, well, it's a 2D engine, right? After getting text rendering working, a change in string caching suddenly broke the entire program. It broke in a particularly bizarre way, because it seemed to just stop rendering halfway through the scene. It took almost an hour of debugging for me to finally confirm that the moment I was rendering a particular text string, the direct2D driver just *stopped*. No errors were thrown. No warnings could be found. Direct2D's failure state was apparently to simply make every single function call silently fail with no indication that it was failing in the first place. It didn't even tell me that the device was missing or that I needed to recreate it. The text render call was made and then every single subsequent call was ignored and the backbuffer was forever frozen to that half-drawn frame.

The error itself didn't seem to make any more sense, either. I was passing a perfectly valid string to Direct2D, but because that string originated in a *different DLL*, it apparently made Direct2D completely shit itself. Copying the string onto the stack, however, worked (which itself could only work if the original string was valid).

The cherry on top of all this is when I discovered that Direct2D's matrix rotation constructor takes **degrees**, not radians, like *every single other mathematical function in the standard library.* [Even DirectX takes radians!](https://msdn.microsoft.com/en-us/library/windows/desktop/bb205362(v=vs.85).aspx)

***WHO WRITES THESE APIs?!***
