+++
title = "WavSaver"
date = 2010-08-25T19:37:00Z
updated = 2011-01-22T03:51:25Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

There is a documented bug in windows 7 that has pissed me off a few times and recently crippled a friend of mine, where a .wav file with corrupted metadata causes explorer.exe to go into an infinite loop. My friend has a large collection of wavs that somehow got corrupted, so I wrote this program to strip them of all metadata. Due to the nature of the bug, the program can't delete them (you must use the command prompt to do that), but rather creates a folder called "safe" with all the stripped wav files inside of it.

Hosted in case anyone else has corrupted wav files they need to save. Just stick it inside a folder and run it - it'll automatically strip all wav files in the same folder as the executable.

[WavSaver](http://www.blackspherestudios.com/storage/WavSaver.zip)
