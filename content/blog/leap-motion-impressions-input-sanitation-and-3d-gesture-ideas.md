+++
title = "Leap Motion Impressions, Input Sanitation, and 3D Gesture Ideas"
date = 2013-07-23T17:05:00Z
updated = 2013-07-23T17:05:15Z
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

{{%blockquote%}}**Pros:**

 * For the most part, does what it claims it does, and gives you extremely precise, fast tracking of fingers.
 
**Cons:**

 * Really hates thumbs for some reason.
 * Has a lot of trouble with pens or other implements.
 * Fingers must be separated.
 * Fairly easy to get positions that break the camera because it can't see the fingers.
 * *No one has any idea how to write software for it.*{{%/blockquote%}}
 
I just got my Leap Motion device today, and for the most part, I like it. It does most of the things I expected it to do. It seems like something that could become very useful down the line. Unfortunately, right now, I wouldn't recommend getting one if you wanted something that's more than a toy.

This isn't entirely the fault of the device itself, but more a combination of driver limitations and boneheaded software that doesn't know what it's doing. The device is so new, no one knows how to properly utilize it, so almost all the apps that exist right now are either garbage or ridiculously sensitive to your setup. There are, however, a few more serious problems that should be addressed by the Leap Motion team.

First of all, the driver hates thumbs. It seems like it's trying to edit them out when they aren't positioned like a finger, which makes any sort of pinching gesture impossible to do. In addition, it doesn't like it when you try to use a pen, despite the fact that it's specifically designed to let you do so. Both of these problems seem more like driver analysis problems, so I expect they'll be corrected soon. I know these aren't hardware limitations, because the camera can see the stupid pen, the driver just has to recognize it as a tool and disregard the hand holding it, which is really doesn't want to do - It's obsessed with hands. I should call it the Lyra driver from now on.

A more fundamental and concerning issue is the ease in which I can assume a position that blocks the camera's view of some or most of my fingers. Pretty much anything that involves my fingers being vertically aligned breaks the driver, along with everything from a roughly 90 degree to 135 degree angle, pointed away from the device. This appears to be an inherent issue with the device itself, and its the same problem the kinect would have, because the camera's view of the fingers gets blocked. How much more effective would a leap motion device with *two* cameras, one on either side of the monitor, be? Or just a wider camera angle (since it's using dual emitters anyway to get depth information). As it is right now, it's cute and futuristic looking, but any minority-report-esque gestures completely break it.

A much more unexpected issue is the fact that the device makes no attempt to deal with fingers that are held together, despite the fact that the shape of two touching fingers is fairly easy to figure out. If you've been tracking one finger and suddenly it meets up with another one and you end up with one really wide "finger", it should be trivial to send this information as a "double finger" pointing, because this is an incredibly useful gesture (see below for details). It's not like the finger is going to change into a hippopotamus all of a sudden.

##### 3D Gestures
Most of the other problems are software oriented, not hardware or driver related. The simple fact is that no one has any idea how to properly utilize the device. Most apps seem to assume there's only going to be one finger pointed at the screen, and will get confused if you have any other fingers hanging off even if they're not pointed at the screen. The Leap Motion driver gives you a rough estimate of what it thinks the hand orientation is, and using this information its fairly trivial to figure out which finger someone is pointing with, even if their other fingers are only slightly below that finger. The camera is on a flat surface at a 90 degree angle from the screen, which means the finger you want is almost always going to be the finger that is the "highest" in relation to the hand orientation.

The rest of the apps either don't do much processing on the raw data, or do an incredibly bad job of it. The touch screen recreation had this horrible tendency to start sliding off, and the cursor would sometimes jump around. I knew it shouldn't be doing that because I've been watching the diagnostic information and it's just getting confused by ghost fingers popping in and out. Guess what? A finger can't teleport. If you've been tracking one finger, chances are you should keep tracking it no matter what else happens.

In addition, no one's really come up with effective 3D gestures. All the apps try to use the 3D plane by turning it into a virtual touchscreen and it doesn't work very well. Instead, we should be using gestures that are independent of a 2D plane. The apps need to be paying attention to the angle the finger is pointing in instead of trying to rely on moving your entire hand around. It seems like everyone is using the position information instead of the angle information, which the driver provides. If I'm pointing up and to the right, my cursor should be in the corner of the screen no matter where my hand is.

Furthermore, no one seems to be trying very hard to deal with noise. When the driver starts losing precision, you can tell because the angle information or position information will start to jitter. The more jittering, the less accurate the reading is. A simple way to quantify this is by calculating the [variance](http://en.wikipedia.org/wiki/Variance) of the recent data. The higher the variance, the less accurate your reading is. You should then scale how heavily you average out the points based on how much variance there is in the data. This allows you to avoid destroying precision while preventing the cursor from exploding when you lose accuracy. There's a lot of algorithms dedicated to solving this exact problem, so it's kind of ridiculous that no one is using them. In a more specific scenario, humans do precise manipulations with their fingers, not their hands. Small pertubations in the input data that occur in the hands should be removed from the finger positions, because it's just the hand trembling, not the actual finger.

Instead of trying to recreate "pushing a button" in 3D, we should use a gesture interface that lets you move the cursor around with your index finger, and then when you bring up your middle finger against your index finger (so you are now pointing at the screen with two fingers), this counts as a "click" or a mouse-down. You can then drag things around with two fingers, or separate them to generate a mouse-up event. To right-click, bring your two fingers together again, and then rotate your hand clockwise. This could be combined with a more traditional "pushing a button" gesture for easier clicks, but provides a much more robust way of moving things around that isn't awkward or difficult to use. 

Since the driver currently doesn't let you track two fingers held together, this same behavior can be emulated by simply keeping track of two fingers and detecting when both of them accelerate towards each other until one of them vanishes. On this same note, because fingers can drop out of tracking, programs can't rely on the driver to be perfect. If you lose track of a finger, it's not that hard to guess where it might be based on the orientation of the hand, which is necessary if we're going to have good gesture recognition. The recognizer needs to be able to interpolate likely positions of lost fingers if it's going to make an effective guess about what gesture was intended.

Leap Motion is really cool. Leap Motion is definitely the future. Unfortunately, we haven't gotten to the future yet. We need a better software framework to build gesture interfaces on top of, and we need a new standard set of gestures for 3D that aren't simply 2D ripoffs. Incidentally, I'd love to try building an open-source library that addresses a lot of the input sanitation problems I outlined in this article, but I'm a bit busy with the whole "finding a job so I don't starve to death" thing. Hopefully, someone else will take up the cause, but until they do, don't expect to be very productive with the Leap Motion.
