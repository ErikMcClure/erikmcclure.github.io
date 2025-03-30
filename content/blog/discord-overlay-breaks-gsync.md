+++
categories = ["blog"]
date = "2025-03-29T17:35:00Z"
title = "The New Discord Overlay Breaks GSync and Borderless Optimizations"
[author]
name = "Erik McClure"

+++

The new discord overlay no longer uses DLL injection, and is instead a permanent `HWND_TOPMOST` window glued to whatever window it happens to think is a game. Ignoring the fact that discord seems to think FL Studio, the minecraft launcher, and SteamVR's desktop widget are "video games", the real problem is that **this breaks the Borderless Windowed Optimizations**, which has the most obvious effect of disabling GSync/FreeSync on all games that the overlay enables itself on.

{{<html>}}<blockquote class="bluesky-embed" data-bluesky-uri="at://did:plc:hdemcqizsy4u42x5ydxga34w/app.bsky.feed.post/3lljyfbtlgs2a" data-bluesky-cid="bafyreiehr7atlqaf7qbv4ztdzkhql2b4o7obc5nqptfwohxyuphqujyw6i" data-bluesky-embed-color-mode="system"><p lang="en">so it seems it also yoinks gsync, which means the game running underneath doesn&#x27;t use gsync anymore. Wonderful!</p>&mdash; Tawmy (<a href="https://bsky.app/profile/did:plc:hdemcqizsy4u42x5ydxga34w?ref_src=embed">@tawmy.dev</a>) <a href="https://bsky.app/profile/did:plc:hdemcqizsy4u42x5ydxga34w/post/3lljyfbtlgs2a?ref_src=embed">March 29, 2025 at 11:45 AM</a></blockquote><script async src="https://embed.bsky.app/static/embed.js" charset="utf-8"></script>{{</html>}}

We can tell that it's a normal window instead of DLL injection by simply finding the window in the win32 UI tree using [inspect.exe](https://learn.microsoft.com/en-us/windows/win32/winauto/inspect-objects):

{{<img src="/img/discord-inspect.png" alt="Discord Overlay Window" >}}

Interestingly, they still seem to be using a D3D window to render the overlay. This might be a quirk of using Electron, or it might be a result of whatever library they're using to render the overlay:

{{<img src="/img/discord-inspect-2.png" alt="Intermediate D3D sibling window?" >}}

The reason this breaks everything is because the borderless windowed optimization works using [a new flip model](https://learn.microsoft.com/en-us/windows/win32/direct3ddxgi/for-best-performance--use-dxgi-flip-model) called the [DXGI flip model](https://learn.microsoft.com/en-us/windows/win32/direct3ddxgi/dxgi-flip-model). Instead of copying the contents of the backbuffer to another intermediate buffer used by the desktop for compositing, the compositor can use the backbuffer directly when it is compositing. This flip model was augmented in Windows 10 with [Direct Flip](https://www.youtube.com/watch?v=E3wTajGZOsA), which allows this shared surface to **bypass the compositor entirely** and send frames to the monitor directly:

{{<blockquote>}} Depending on window and buffer configuration, it is possible to bypass desktop composition entirely and directly send application frames to the screen, in the same way that exclusive fullscreen does. {{</blockquote>}}

All modern gaming is built on top of this key optimization, because it allows seamless Alt-Tab behavior by allowing the DWM compositor to "wake up" and start compositing the screen like a normal application, then "go to sleep" once it knows a single borderless fullscreen application is the only thing rendering to that monitor, by simply piping it's backbuffer directly to the device. If a combination of `DXGI_FEATURE_PRESENT_ALLOW_TEARING` and the right VSync mode is enabled, the app can update it's backbuffer completely out-of-band from the rest of the desktop compositor, which is the only thing that allows GSync/FreeSync to work, as the monitor must sync it's own refresh rate to whenever the game happens to complete a frame.

If any part of this pipeline is disrupted, it is no longer possible to forward frames to the monitor outside the normal update sequence of the compositor. *Many* things can break this, like [not turning on optimizations for windowed games](https://support.microsoft.com/en-us/windows/optimizations-for-windowed-games-in-windows-11-3f006843-2c7e-4ed0-9a5e-f9389e535952) or having windows fail to recognize something as a game. If the vsync mode is set up wrong, it will break. If the flip mode is wrong, it will break. And most importantly, if even *a single pixel of another app* is displayed over the game, then in order to display that pixel, the compositor has to composite the window outputs together onto a secondary buffer, which must then be presented at the native refresh rate of the monitor because it has inputs from two different programs at different refresh rates, thus breaking GSync/FreeSync. It will also introduce additional frames of lag even if you don't use GSync/FreeSync, which you may have noticed when a notification pops up while playing a game and it suddenly felt laggy until the notification went away.

DLL Injection was originally used for in-game overlays because games often used exclusive fullscreen. The drawback of DLL injection is that it crashes games when implemented incorrectly and also makes virus scanners very unhappy. With the new flip models, games don't need to ask for exclusive fullscreen to get low latency, but **they still have to be the only thing on the screen or it doesn't work**. Discord has either ignored why DLL injection was originally used, or decided that the drawbacks of DLL injection aren't worth it and instead simply broken all the optimizations for windowed games that Microsoft introduced. Any half-decent graphics programmer would know this would happen, so it's obvious that one of two things happened:

1) Discord never involved a single graphics dev or gamedev with any experience in how games work about how their new overlay *for games* would interact *with games*.
2) There is a very angry dev stalking the halls of discord HQ right now, cursing at the shadows because she knew. SHE KNEW. *SHE WARNED THEM*. **BUT THEY DIDN'T LISTEN**. Her manager probably ignored her warnings, or overruled them, saying "most gamers won't even notice" or "DLL Injection has too many problems". And now, if she shares this article with said manager, her manager will look bad and probably try to fire her for the crime of being competent, because that's how big corporations work.

Usually I default to option (2), but option (1) is also possible if they already laid off the person who knew this would happen [last year](https://www.theverge.com/2024/1/11/24034705/discord-layoffs-17-percent-employees). But hey, if you are a graphics dev at discord who tried to warn your managers about this trashfire and you got "laid off" under mysterious circumstances, send me a DM on bluesky, I'm always interested in talking to actual competent engineers.

If the new overlay was turned on without your consent (which is what happened to me), you can turn it off again by going to **User Settings → Activity Settings → Game Overlay → Enable Overlay**. If you want to uninstall Discord, you can do so from Add/Remove programs, but good luck finding another chat app your friends actually want to use.