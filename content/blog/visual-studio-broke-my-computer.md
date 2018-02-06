+++
blogimport = true
categories = ["blog"]
date = "2012-03-10T16:22:00Z"
title = "Visual Studio Broke My Computer"
updated = "2012-03-11T13:39:54.000+00:00"
comments = [ 2388425063295457664, 7255547481340553456 ]
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
So I'd been using the developer preview of VS11 and liked some of its improvements. When the desaturated VS11 beta came out, [I hated the color scheme](http://blackhole12.blogspot.com/2012/02/implicit-ui-design.html) but decided I still wanted the upgraded components, so I went to install VS11 beta. Unfortunately the beta only lets you change its install location if the preview developer preview isn't installed, and the developer preview had installed itself into C:\ without ever letting me change the path, which was annoying. So I took the opportunity to fix things and uninstalled the developer preview, then installed the beta of VS11.

Everything was fine and dandy until I discovered that VS11 wasn't compiling C++ DLLs that worked on XP. I don't know how it managed to do this, since the DLL had no dependencies whatsoever, and that bug was only supposed to affect MFC and other windows related components and hence there was no windows flag for me to specify which version I wanted, but just to be sure I decided to try and compile it in VS2010. It was at this point I discovered that VS2010 could no longer open any projects at all. It was broken. Further investigation revealed that uninstalling VS11 developer preview will break VS2010. Now, I had an ultimate version of VS2010 I've had sitting around for a while I got from Dreamspark, so I figured I could just uninstall VS2010 and then reinstall the ultimate version and that would kill any remaining problems the pesky 2011 beta introduced.

The thing is, I can't uninstall the SP1 update from VS2010. Not before I uninstalled VS2010, not after I uninstalled it, not even after I installed the ultimate version. It just gave me this:

{{%blockquote%}}*The removal of Microsoft Visual Studio 2010 Service Pack 1 may put this computer in an state in which projects cannot be loaded and Service Pack 1 cannot be reinstalled. For instructions about how to correct the problem, see the readme on the Microsoft Download Center website.*{{%/blockquote%}}

So I just had to leave the Service Pack alone and attempted to re-apply it after installing VS2010 Ultimate, but the online installer failed. So then I downloaded the SP1 iso file and installed that. It failed too, but this time I could fix the problem - someone had forgotten to copy the F#_redist MSI file to the TEMP directory, instead only copying the CAB file. Note that I don't even have F# installed.

I was able to resolve that problem and finished installing the service pack, but to no avail. Both the VS2010 installation and the service pack had forgotten to install the C++ standard library headers, which, as you can imagine, are kind of important. I searched around for a solution, but the only guy who had the same problem as me had simply [reformatted and reinstalled windows](http://social.msdn.microsoft.com/Forums/pl/vssetup/thread/86743068-8e78-4f20-bb3e-44ff0e5170c0) (note the moderator's excellent grasp of english grammar). The only thing I had to go off of was using a [special utility](http://archive.msdn.microsoft.com/vs2010uninstall) they built to uninstall all traces of VS2010 from your computer. Unfortunately, the utility doesn't actually succeed in uninstalling everything, and also doesn't uninstall SP1, so you have to uninstall SP1 first before running the utility. The problem is, I can't uninstall SP1 or I'll never be able to install it again.

At this point it appears I am pretty much fucked. How does Microsoft consider this an acceptable scenario? I worked as an intern at Microsoft once, I *know* they use their own development tools. I used tools that hadn't even been released yet. There was one guy on our team whose entire job was just the setup. And yet, through a series of astonishingly bad failures, any one of which being fixed would have prevented this scenario, my computer is now apparently fucked, and I'm going to have to upgrade my windows installation to 64 bit a lot sooner than I wanted.

**EDIT:** After using the uninstall tool to do a full uninstall and uninstalling SP1 and manually finding any VC10 related registry entries in the registry and deleting them, then reinstalling everything from scratch, I solved the header file problem (but had to reinstall SP1 or it wouldn't let me open my project files). However then the broken VCTargetsPath problem showed up again, which a repair didn't fix. I finally fixed the issue by finding someone else with a working installation of VC10, having them export their MSBuild registry key and manually merging it into my registry. If you have this problem, I've uploaded the registry key (which should be the same for any system, XP or 7) [here](http://www.mediafire.com/?91yph5n2704hvzi). If you have a 64-bit machine, you may need to copy its values into the corresponding WoW64 nodes (just search for a second instance of MSBuild in your registry).
