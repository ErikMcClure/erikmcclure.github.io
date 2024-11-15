+++
blogimport = true
categories = ["blog"]
comments = [4751228645406544520, 5911677390758794331, 5395726161580931704]
date = "2015-07-21T00:45:00Z"
title = "I Tried To Install Linux And Now I Regret Everything"
updated = "2015-07-21T09:48:17.000+00:00"
draft = true
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
<style>.imghover img { display:none; position:absolute; margin-top:-350px; right:0; background: white; border: 1px solid black; } .imghover:hover img, .imghover:focus img { float:left; display:block; }</style>I am going to tell you a story.

This story began with me getting pissed off at Windows for reasons that don't really need to be articulated. Just, pick your favorite reason to hate Windows, and let's pretend that was the tipping point. I had some spare space on my secondary drive, so I decided to give Linux another whirl. After all, it had been three or four years since I last attempted anything like this (it's been so long I don't rightly remember the last time I tried), and people have been saying that Linux has gotten a lot better recently!

The *primary* reason I was attempting this is because of [Valve's attempts to move to linux](http://store.steampowered.com/steamos), which have directly resulted in much better nVidia driver support. Trying to install proper nVidia drivers is the thing that wrecked my last attempt. This time, I figured, I could just install the nVidia drivers straight from the repo and everything would be hunky dory, and I could just spend the rest of my time beating on the rest of linux with a comically oversized wrench until it did what I wanted.

I'd had good experiences with [XFCE](http://www.xfce.org/) and [Fedora](https://getfedora.org/) on my Linux VM, but I didn't really like Fedora itself (but it interfaced very well with my VM environment). I wanted to install [Ubuntu](http://www.ubuntu.com/), because it has the best support and I don't like trying to dig through arcane forum posts trying to figure out why my computer screen has suddenly turned into an invisible pink unicorn. Unfortunately, Ubuntu is a bloated mess, and I *hate* it's default desktop environment. In the past, I had tried Linux Mint, which had been okay, but support had been shaky. I spotted [Lubuntu](http://lubuntu.net/), which is supposed to be a lightweight ubuntu on top of LXDE, a minimal window manager similar to XFCE. This was perfect! So I downloaded Lubuntu 15.04 and installed it on my secondary drive and everything was nice and peachy.

Well, until Linux started, anyway. The first problem was that the package manager in pretty much every linux distro lists *all* the packages, including all the ones I don't understand. I was trying to remove some pre-included Paint application and it had two separate packages, one named after itself, and one named {{<code>}}<name>-common{{</code>}}, but the common package wasn't automatically removed when the program was! The nVidia packages also had both an nvidia-drivers-340{{<sup>}}<a href="#footnote1">[1]</a>{{</sup>}} package, and an nvidia-drivers-340-update package, both of which had identical descriptions. I just went with the most basic one because it seemed sensible, but I felt really sorry for someone less tech savvy trying to find *anything* in that godforsaken list.

So after everything updated and I restarted and told the package manager to start installing my nVidia drivers, I started noticing annoying things about LXDE. A lot of annoying things. Here, let me list them for you:

 * The file manager, when unmounting anything, would helpfully close itself.
 * Trying to change the time to not display military time involves editing some arcane string whose only value is {{<code>}}%r{{</code>}}, and I still don't know what that means and don't want to know what that means. All I wanted to do was change it to say AM or PM!
 * In order to get a shortcut onto the desktop, you had to *right-click the menu item* and then a new menu would show up that would let you add it to the desktop. You can't drag anything out of the start menu.
 * The shortcuts seemed disturbingly fickle, occasionally taking 4 rapid clicks to get things to start up.
 * Steam simply didn't start *at all*. Unfortunately, we will never know why.
 * Skype managed to spawn a window halfway above the top of the screen, which resulted in me having to look up {{<code>}}alt+space{{</code>}} in order to rescue it.
 * Skype also cut off everything below the baseline of the bottom-most line of text, so you couldn't tell if something was an i or a j.
 * Like in Windows, in Linux, modal dialogs have a really bad habit of sucking up keyboard focus, focusing on a button, and then making my next keystroke click the button. The fact that this is a widespread UI problem across most operating systems is just silly.
 
There were more nitpicks, but I felt like a large number of these issues could have probably be resolved by switching to a better window manager. I was honestly expecting better than this, though. LXDE was *supposed* to be like XFCE, but apparently it's actually XCFE with all it's redeeming qualities removed. However, it turns out that *this doesn't matter!* To discover why, we have to examine what happened next.

My friend suggested to get Linux Mint, which has a better window manager and would probably address most of those issues. So, I downloaded the ISO. I already had a USB set up for booting that had Lubuntu on it, so I wanted to see if I could just extract the contents of the ISO and put them on the USB stick. I have no idea if this would have actually worked or not, and I never got to find out because upon clicking and dragging the contents of the ISO out of the archive manager, with the intent of extracting them, *the entire system locked up*.

Now, I'm not sure how stupid trying to drag a file out of an ISO was, but I'm pretty sure it shouldn't *render my entire computer unusable*. This seems like an unfair punishment. Attempts to recover the desktop utterly failed, as did {{<code>}}ctrl-alt-del{{</code>}}, {{<code>}}alt-F2{{</code>}}, {{<code>}}alt-{{</code>}}anything else, or even trying to open a terminal (my friend would later inform me that it is actually {{<code>}}*ctrl-alt-F2*{{</code>}} that opens the terminal, but I couldn't ask him because *the entire desktop had locked up!*). So I just restarted the machine.

That was the last time I saw my Lubuntu desktop.

Upon restarting my machine, I saw the Lubuntu loading screen come up, and then... nothing. Blackness. About 20 seconds later, an error message pops up: "Soft Lock on CPU#0!". Then the machine rebooted.

After spending just *one hour* using linux, it had bricked itself.{{<sup>}}<a href="#footnote2">[2]</a>{{</sup>}} I hadn't even gotten steam working yet, and now it was completely unusable. This is not "getting better", this is strapping a rocket to your ass and going so fast in the wrong direction you break the sound barrier. Now, if you have been paying attention, you will note that I had just finished installing my nVidia drivers before the desktop locked up, and that the drivers probably wouldn't actually be used by the desktop environment until the process was restarted. After mounting my linux partition in windows and extracting my log files, I sent them to my friend, who discovered that it had indeed been the nVidia driver that had crashed the system.{{<sup>}}<a href="#footnote3">[3]</a>{{</sup>}}

This is problematic for a number of reasons, because those drivers were written for Ubuntu based distros, which means my system could potentially lock up if I installed any other ubuntu based distro, which is... just about all the ones that I cared about. Including Linux Mint. At this point, I had a few options:

1) Install another ubuntu based distro anyway and hope it was a fluke.
2) Install something based on Debian or maybe Fedora.
3) Use the open-source drivers.
4) *[Fuck this shit.](https://www.youtube.com/watch?v=5FjWe31S_0g)*

Unfortunately, I have little patience left at this point, and after seeing Linux first *lock up after trying to drag files* before *bricking itself*, I don't really have much confidence in the reverse-engineered open-source nVidia drivers, and I certainly am not going to entrust my video card to them in the hopes it doesn't melt. I really, *really* don't want to play whack-a-mole with Linux distros, trying to find the magical one that *wouldn't* wreck my graphics card, so I have simply opted for option 4.

But it doesn't end there.

About two or three hours after this all happened (I had been fairly distracted, and by distracted I mean {{<span class="imghover">}}<img src="https://googledrive.com/host/0B_2aDNVL_NGmUlp0bnh3Z3hUeWc/">}}<a href="javascript:void()">blinded by rage</a>{{</span>}}), I noticed that my windows clock was way off. At this point, I remembered something my friend had told me about - Linux, like any sane operating system would, sets the hardware clock to UTC and then modifies it based on the timezone. Windows, on the other hand, decided it would be a fantastic idea to set the hardware clock to *local time*. Of course, this should have been an easy fix. I just set the time back, forced an update, and bam, time was accurate again. Crisis averted, right?

No, of course not. Linux had not yet finished punishing me for foolishly believing I was worthy of installing something of it's calibre. Because then I opened Skype, and started receiving messages *in the past*. Or more accurately, my chat logs were now full of messages that had been sent *tomorrow*.

It was at this point I realized what had happened. It wasn't that the timezone had been changed, or something reversible like that. The *hardware clock itself* had been modified to an incorrect value. Skype had spent the past two hours happily sending messages with a timestamp *8 hours in the future* because some idiot at Microsoft thought it was a good idea to set the hardware clock to local time, and now all these incorrect client side timestamps had been propagated to the cloud and synced across all my devices.

I slowly got up from my computer. I walked over to my bed, lied down, curled into a ball, and wept for the future of mankind.

{{<html>}}<center><iframe width="420" height="315" src="https://www.youtube.com/embed/HIWHMb3JxmE" frameborder="0" allowfullscreen></iframe></center>
<hr>{{</html>}}
{{<sup>}}<a name="footnote1">1</a>{{</sup>}} I may have gotten these package names slightly wrong, but I can't verify them because *Linux won't boot up!*
{{<sup>}}<a name="footnote2">2</a>{{</sup>}} It's probably entirely possible to rescue the system by mounting the file system and modifying the x config file to not use nvidia, but this was supposed to just be "install linux and set it up", not "install linux and spend the next 5 hours frantically trying to get it to boot properly."
{{<sup>}}<a name="footnote3">3</a>{{</sup>}} After submitting my log files to #ubuntu to report the issue, my friend discovered that the drivers for ATI/AMD graphics drivers are also currently broken for many ubuntu users. What timing! This certainly makes me feel confident about using this operating system!
