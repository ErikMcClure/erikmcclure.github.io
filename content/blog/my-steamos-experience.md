+++
blogimport = true
categories = ["blog"]
date = "2013-12-14T22:46:00Z"
title = "My SteamOS Experience"
updated = "2013-12-14T23:55:19.000+00:00"
comments = [ 7312333325272052536 ]
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{<div style="text-align:center">}}<a href="http://i.imgur.com/UoeZCWN.png"><img style="max-height:256px;" src="http://i.imgur.com/UoeZCWN.png" alt="SteamOS"/></a>{{</div>}}
After quite a bit of fighting, I have managed to create a stable, working SteamOS installation, on which I am writing this blog post. In the interest of ***science!*** I will be detailing the steps I took while installing the OS, along with all the issues I had and how I solved them. To help anyone trying to troubleshoot things, here are a list of things that went wrong. If you want to know how they were fixed, keep reading:

 * Initial steam shortcut failed to start the program
 * Recovery partition failed to install and wouldn't shut down properly
 * SteamOS cannot be used with more than 1 monitor!
 * Upon login, steam requires a confirmation code. That's sent to your e-mail. Which you need a web browser for.
 * Steam doesn't have any default repositories
 * Sound doesn't work
 
To install SteamOS, I essentially followed the steps [outlined here](http://www.reddit.com/r/SteamOS/comments/1swy8a/steam_os_full_installation_guide_uefi_workaround/), but they weren't all nicely organized for me. I downloaded the zip, extracted it into a FAT32 usb, copied the syslinux files, executed syslinux on the drive, and wrote syslinux.cfg, which is improperly formatted in that post. Here is the proper formatting:

{{<pre>}}DEFAULT linux
TIMEOUT 50
LABEL linux
    kernel install.amd/vmlinuz
    append initrd=install.amd/gtk/initrd.gz preseed/file=/cdrom/default.preseed DEBCONF_DEBUG=developer desktop=steamos auto=true priority=critical video=vesa:ywrap,mtrr vga=788 -- quiet
{{</pre>}}
At this point I had done everything I needed to do from windows. SteamOS cannot be dual-booted, because it obliterates whatever drives are connected to the computer. To work around this, I used a second hard drive and disconnected the first one. To avoid any mistakes, I physically disconnected the drive from the computer to ensure it would be safe. Only after doing that and disabling it in the BIOS did I configure the USB to boot up.

{{<span style="color:#f00">}}**WARNING: DO *NOT* CONFIGURE YOUR BIOS TO BOOT FROM USB UNTIL ALL OTHER HARD DISKS HAVE BEEN REMOVED FROM YOUR SYSTEM. THERE IS NO CONFIRMATION DIALOG. THERE IS NO BUTTON THAT SAYS "PRESS OK TO IRREVERSIBLY DESTROY ABSOLUTELY EVERYTHING ON THIS SYSTEM." THE MOMENT IT BOOTS OFF THAT USB, ANYTHING CONNECTED TO THE COMPUTER WILL BE DESTROYED.**{{</span>}}

As expected, the installer failed when installing grub. I dropped into the terminal and typed in the magic instructions, which were a lot easier to do after I realized I could just hit tab and have the terminal complete the filenames for me. Once I got back into the setup, though, the guide didn't mention that you actually have to hit continue 3 or 4 times. However, it did indeed work on the second try, and I soon found myself booting into a GNOME environment.

It's important that, when first logging in, you pick the GNOME shell environment. It isn't the default, and it's not the SteamOS environment. Failure to do so will fuck everything up really bad. However, once I did get into the GNOME shell environment, the steam shortcut *didn't work*. I don't know why, but I had to go into the terminal and manually execute {{<code>}}/usr/bin/steam %U{{</code>}}. This is exactly what the shortcut did, but for some reason it only worked when I typed it into the terminal. Once steam was installed, I logged off and logged into the desktop account (password: desktop). I was able to run {{<code>}}~/post_logon.sh{{</code>}} without a hitch, and the system rebooted.

Then, the recovery creator failed. It failed *spectacularly*. It spat out a bunch of errors, tried to continue, then dropped into a terminal. I told it to restart, but it *failed.* It kept reverting back to the terminal because it somehow couldn't disconnect the terminal session despite me telling it to *shut-the-fuck-down-right-now-or-else*. So I just held the power button down and did a hard shutdown. Once I rebooted, despite not having a recovery partition, it seemed to work just fine. It booted into steamOS and then...

Nothing. Black screen. Complete failure.

This, however, turns out to simply be what happens when Steam's compositor encounters two monitors. Instead of shutting one off, it just completely dies. After unplugging my second monitor, steam worked like a charm, and I logged in, and then... I was prompted to enter a confirmation code sent to my e-mail. Which I needed a web browser to get to, which required that *I logged in*. Thankfully, I had a laptop sitting next to me, so this wasn't a problem, but fair warning to anyone else who's logging in.

Now I was in the Steam client, but I had no sound. Dropping back to desktop was easy after checking the "Enable Linux Desktop" option under "Interface", but I had to assign a new password while in desktop mode so I could use {{<code>}}sudo{{</code>}} (this is done using the {{<code>}}passwd{{</code>}} command).

The problem is that steam's only default repository is for, well, steam. To get anything else that would make my OS usable, like *have sound*, I needed to add back in the default debian repositories. SteamOS thankfully ships with nano, so I typed {{<code>}}sudo nano /etc/apt/sources.list{{</code>}} and added the following:
{{<pre>}}deb http://http.debian.net/debian wheezy main
deb-src http://http.debian.net/debian wheezy main{{</pre>}}
Now, after running {{<code>}}sudo apt-get update{{</code>}}, I had access to the default packages, which crucially included ALSA, the linux audio API and low-level driver package. After installing and restarting ALSA, however, I still didn't have sound. Sometimes when this occurs, it's recommended to open a console and run {{<code>}}rm -rf ~/.pulse{{</code>}} to delete the .pulse configuration files, which can mess things up. In my case, however, the hardware still wasn't being detected. To solve this, I installed {{<code>}}alsa-firmware-loaders{{</code>}}. After rebooting, my sound *still* didn't work, so I ran {{<code>}}alsa force-reload{{</code>}}. This finally did the trick, and I had sound on the desktop! But, not on the steam client, or the games.

Rebooting finally graced me with sound in the steam client, but disabled sound on the *desktop*. So, now I have sound on Steam, but not on the desktop. This isn't a big problem right now, but hopefully someone can fix it eventually.

Unfortunately, I could not get flash working, and debian comes with a firefox fork, which doesn't support whatever audio API everyone wants to use right now. Luckily, now that we have the default debian packages, we can install things like [GNOME tweaker](http://steamcommunity.com/groups/steamuniverse/discussions/1/648814395929616740/), and other fun things to customize our steamOS experience.

Once I got sound working, I tried out a few games. SpaceChem did not appear to have a fully-working linux client, because it failed to sync my saves from steam cloud, and half it's menu buttons were just gone. Team Fortress 2 worked fine, aside from awkward looking text, but it was running at about 2 FPS until I disabled motion blur. After doing that, it abruptly started running smoothly, with just a few hiccups here and there, so I don't know what that was about.

My computer only has 4 GB of RAM, so TF2 normally has memory issues on windows, which usually cause temporary freezeups. On steamOS, those memory problems still exist, but instead of causing temporary freezeups, the entire game instantly crashes. Given that this is a failure case, it's not really something I'm allowed to complain about, but be warned: don't run out of RAM.

Aside from the occasional whacky interface, playing games on SteamOS feels almost identical to playing them on windows. Performance and load times appear to be identical to windows, controls are responsive, everything *usually* works. While this is only a beta, I consider this a step in the right direction, and am optimistic about the future of SteamOS.
