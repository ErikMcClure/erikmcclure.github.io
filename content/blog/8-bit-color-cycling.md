+++
title = "8-bit color cycling"
date = 2010-08-10T07:58:00Z
updated = 2011-01-22T03:53:04Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Someone linked me to this awesome webpage that uses HTML5 to do 8-bit palette color cycling using Mark Ferrari's technique and art. I immediately wanted to implement it in my graphics engine, but soon realized that the technique is so damn old that no modern graphics card supports it anymore. So, I have come up with a pixel shader that creates the same functionality, either by having one image with an alpha channel containing the palette indices and a separate texture acting as the palette, or you can combine them into a single image. This is supposed to support variable palette sizes (up to 256) but I haven't had much ability to test the thing because its so damn hard to get the images formatted correctly. So while all of these variations i'm about to show you should work there is no guarantee they necessarily will.

Video Link

8-bit cycling multi-image

{{<pre cpp>}}ps 2.0 HLSL
// Global variables
float frame;
float xdim;
float xoff;

// Samplers
sampler s0 : register(s0);
sampler s1 : register(s1);

float4 ps_main( float2 texCoord : TEXCOORD0 ) : COLOR0
{
float4 mainlookup = tex2D( s0, texCoord );
float2 palette = float2(mainlookup.a*xdim + xoff,frame);
mainlookup = tex2D(s1, palette);
return mainlookup;
}

 
ps 1.4 ASM
ps.1.4
texld r0, t0
mad r0.x, r0.a, c1, c2
mov r0.y, c0
phase
texld r1, r0
mov r0, r1
 
It is also possible to write the shader in ps.1.1 but it requires crazy UV coordinate hacks.

frame is a value from 0.0 to 1.0 (ps.1.4 will not allow you to wrap this value, but ps.2.0 will) that specifies how far through the palette animation you are.
xdim = 255/(width of palette)
xoff = 1/(2*(width of palette))

Note that all assembly registers correspond to a variable in order of its declaration. So, c0 = frame, c1 = xdim, c2 = xoff.

8-bit cycling single-image

ps 2.0 HLSL
// Global variables
float frame;
float xdim;
float xoff;

// Samplers
sampler s0 : register(s0);

float4 ps_main( float2 texCoord : TEXCOORD0 ) : COLOR0
{
float4 mainlookup = tex2D(s0, texCoord );
float2 palette = float2(mainlookup.a*xdim + xoff,frame);
mainlookup = tex2D(s0, palette);
mainlookup.a = 1.0f;
return mainlookup;
}

 
ps 1.4 ASM
ps.1.4
def c3, 1.0, 1.0, 1.0, 1.0
texld r0, t0
mov r1, r0
mad r1.x, r1.a, c1, c2
mov r1.y, c0
phase
texld r0, r1
mov r0.a, c3
{{</pre>}}
 
frame is now a value between 0.0 and (palette height)/(image height).
xdim = 255/(image width)
xoff = 1/((image width)*2)

24-bit cycling

{{<pre cpp>}}ps 2.0 HLSL
// Global variables
float frame;
float xdim;
float xoff;

// Samplers
sampler s0 : register(s0);
sampler s1 : register(s1);

float4 ps_main( float2 texCoord : TEXCOORD0 ) : COLOR0
{
float4 mainlookup = tex2D( s0, texCoord );
float2 palette = float2(mainlookup.a*xdim + xoff,frame*fElapsedTime);
float4 lookup = tex2D(s1, palette);
return lerp(mainlookup,lookup,lookup.a);
}

 
ps 1.4 ASM
ps.1.4
def c3, 1.0, 1.0, 1.0, 1.0
texld r0, t0
mov r2, c0
mad r2.x, r0.a, c1, c2
phase
texld r1, r2
mov r0.a, c3
lrp r0, r1.a, r1, r0
{{</pre>}}
 
Variables are same as 8-bit multi-image.

All this variation does is make it possible to read an alpha value off of the palette texture, which is then interpolated between the palette and the original color value. This way, you can specify 0 alpha palette indexes to have full 24-bit color, and then just use the palette swapping for small, animated areas.

If I had infinite time, I'd write a program that analyzed a palette based image and re-assigned all of the color indexes based on proximety, which would make animating using this method much easier. This will stay as a proof of concept until I get some non-copyrighted images to play with, at which point I'll probably throw an implementation of it inside my engine.
