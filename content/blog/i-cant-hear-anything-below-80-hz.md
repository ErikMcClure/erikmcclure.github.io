+++
title = "I Can't Hear Anything Below 80 Hz*"
date = 2017-03-07T15:06:00Z
updated = 2017-03-12T19:03:32Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

{{%blockquote%}}** at a comfortable listening volume.*{{%/blockquote%}}

**EDIT: I have confirmed all the results presented here by taking the low frequency test with someone standing physically next to me. They heard a tone beginning at 30 Hz, and by the time I could hear a very faint tone around 70 Hz, they described the tone as "conversation volume level", which is about 60 dB. I did not reach this perceived volume level until about 120 Hz, which strongly correlates with the experiment. More specific results would require a professional hearing test.**

For almost 10 years, I've suspected that something was wrong with my ability to hear bass tones. Unfortunately, while everyone is used to people having difficulty hearing high tones, nobody takes you seriously if you tell them you have difficulty hearing *low* tones, because most audio equipment has shitty bass response, and human hearing isn't very precise at those frequencies in the first place. People generally say "oh you're just supposed to *feel* the bass, don't worry about it." This was extremely frustrating, because one of my hobbies is writing music, and I have struggled for years and years to do proper bass mixing, which is basically the only activity on the entire planet that actually requires hearing subtle changes in bass frequencies. This is aggravated by the fact that most hearing tests are designed to detect issues with high frequencies, not low frequencies, so all the basic hearing tests I took at school gave test results back that said "perfectly normal". Since I now have professional studio monitor speakers, I'm going to use science to prove that I have an abnormal frequency sensitivity curve that severely hampers my ability to differentiate bass tones. Unfortunately, at the moment I live alone and nowhere near anyone else, so I will have to prove that my equipment is not malfunctioning without being able to actually hear it.

Before performing the experiment, I did [this simple test](http://www.audiocheck.net/audiotests_frequencychecklow.php) as a sanity check. At a normal volume level, I start to hear a very faint tone in that example at about 70 Hz. When I sent it to several other people, they all reported hearing a tone around 20-40 Hz, even when using consumer-grade hardware. This is clear evidence that something is very, very wrong, but I have to prove that my hardware is not malfunctioning before I can definitively state that I have a problem with my hearing.

For this experiment, I will be using two JBL Professional LSR305 studio monitors plugged into a Focusrite Scarlett 2i2. Since these are studio monitors, they should have a roughly linear response all the way down to 20 Hz. I'm going to use a free sound pressure app on my android phone to prove that they have a relatively linear response time. The app isn't suitable for measuring very quiet or very loud sounds, but we won't be measuring anything past 75 dB in this experiment because I don't want to piss off my neighbors.

{{<img src="/img/chart1.png" alt="Speaker Frequency Response Graph" width="700" >}}

The studio monitor manages to put out relatively stable noise levels until it appears to fall off at 50 Hz. However, when I played a tone of 30 Hz at a volume loud enough for me to *feel*, the sound monitor still reported no pressure, which means the *microphone* can't detect anything lower than 50 Hz (I was later able to prove that the studio monitor is working properly when someone came to visit). Of course, I can't hear anything below 50 Hz anyway, no matter how loud it is, so this won't be a problem for our tests. To compensate for the variance in the frequency response volume, I use the sound pressure app to measure the *actual sound intensity* being emitted by the speakers.

The first part of the experiment will detect the softest volume at which I can detect a tone at any frequency, starting from D3 (293 Hz) and working down note by note. The loudness of the tone is measured using the sound pressure app. For frequencies above 200 Hz, I can detect tones at volumes only slightly above the background noise in my apartment (15 dB). By the time we reach 50 Hz I was unwilling to go any louder (and the microphone would have stopped working anyway), but this is already enough for us to establish 50 Hz as the absolute limit of my hearing ability under normal circumstances.

{{<img src="/img/chart2.png" alt="Threshold of Hearing Graph" width="700" >}}

To get a better idea of my frequency response at more reasonable volumes, I began with a D4 (293 Hz) tone playing at a volume that corresponded to 43 dB SPL on my app, and then recorded the sound pressure level of each note once it's volume seemed to match with the other notes. This gives me a rough approximation of the 40 phon [equal loudness curve](https://en.wikipedia.org/wiki/Equal-loudness_contour), and allows me to overlay that curve on to the ISO 226:2003 standard:

{{<img src="/img/chart3.png" alt="Equal Loudness Contour" width="700" >}}

These curves make it painfully obvious that my hearing is severely compromised below 120 Hz, and becomes nonexistent past 50 Hz. Because I can still technically *hear* bass at extremely loud volumes, I can pass a hearing test trying to determine if I *can* hear low tones, but the instant the tones are not presented in isolation, they are drowned out by higher frequencies due to my impaired sensitivity. Because all instruments that aren't pure sine waves produce harmonics above the fundamental frequency, this means the only thing I'm hearing when a sub-bass is playing *are the high frequency harmonics*. Even then, I can still *feel* bass if it's loud enough, so the bass experience isn't completely ruined for me, but it makes mixing almost impossible because of how bass frequencies interact with the waveform. Bass frequencies take up lots of headroom, which is why in a trance track, you can tell where the kicks are just by looking at the waveform itself:

{{<img src="/img/bass_example.png" alt="Bass Example" width="700" >}}

When mixing, you must carefully balance the bass with the rest of the track. If you have too much bass, it will overwhelm the rest of the frequencies. Because of this, when I send my tracks to friends to get help on mixing, I can tell that the track sounds better, but I can't tell why. The reason is because they are adjusting bass frequencies *I literally cannot hear*. All I can hear is the end result, which has less frequency crowding, which makes the higher frequencies sound better, even though I can't hear any other difference in the track, so it seemes like black magic.

It's even worse because I am almost completely incapable of differentiating tones below 120 Hz. You can play any note below B2 and I either won't be able to hear it or it'll sound the same as all the other notes. I can only consistently differentiate semitones above 400 Hz. Between 120-400 Hz, I can sometimes tell them apart, but only when playing them in total isolation. When they're embedded in a song, it's hopeless. This is why, in AP Music Theory, I was able to perfectly transcribe all the notes in the 4-part writing, *except the bass*, yet no other students seemed to have this problem. My impaired sensitivity to low frequencies mean they get drowned out by higher frequencies, making it more and more difficult to differentiate bass notes. In fact, in most rock songs, I can't hear the bass guitar *at all*. The only way for me to hear the bass guitar is for it to be played by itself.

Incidentally, this is probably why I hate dubstep.

For testing purposes, I've used the results of my sensitivity testing to create an EQ filter that mimics my hearing problems as best I can. I can't tell if the filter is on or off. For those of you that use FL Studio, the preset can be [downloaded here](https://drive.google.com/file/d/0B_2aDNVL_NGmZEFkcFdlNmVxN1U/view?usp=sharing).

{{<img src="/img/EQ_demo.png" alt="EQ Curve">}}

Below is a song I wrote some time ago that was mastered by a friend who can actually hear bass, so hopefully the bass frequencies in this are relatively normal. I actually have a bass synth in this song I can only *barely* hear, and had to rely almost entirely on the sequencer to know which notes were which.

{{<html>}}<iframe width="100%" height="150" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/213925837&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&visual=true"></iframe>{{</html>}}

This is the same song with the filter applied:

{{<html>}}<iframe width="100%" height="160" src="https://clyp.it/4fkctaiv/widget" frameborder="0"></iframe>{{</html>}}

By inverting this filter, I can attempt to "correct" for my bass hearing, although this is only effective down to about 70 Hz, which unfortunately means the entire sub-bass spectrum is simply inaudible to me. To accomplish this, I combine the inverted filter with a mastering plugin that completely removes all frequencies below 60 Hz (because I can't hear them) and then lowers the volume by about 8 dB so the amplified bass doesn't blow the waveform up. This doesn't seem to produce any audible effect on songs without significant bass, but when I tried it on a professionally mastered trance song, I was able to hear a small difference in the bass kick. I also tried it on [Brothers In Arms](https://www.youtube.com/watch?v=xllG3fSUAOw) and, for the first time, noticed a very faint bass cello going on that I had never heard before. If you are interested, the FL studio mixer state track that applies the corrective filter is [available here](https://drive.google.com/file/d/0B_2aDNVL_NGmSWJEbE9YVGdrdnM/view?usp=sharing), but for normal human beings the resulting bass is probably offensively loud. For that same reason, it is unfortunately impractical for me to use, because listening to bass frequencies at near 70 dB levels is bad for your hearing, and for that matter it doesn't fix my impaired fidelity anyway, but at least I now know why bass mixing has been so difficult for me over the years.

I guess if I'm going to continue trying to write music, I need to team up with one of my friends that can actually hear bass.
