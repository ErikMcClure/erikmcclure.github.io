+++
blogimport = true
categories = ["blog"]
date = "2009-12-03T18:21:00Z"
draft = true
title = "Turning a 3D animation into a 2D animation"
updated = "2011-01-22T04:29:08.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I don't have anyone on MSN to excitedly spew out this idea to so I'll just do this as a journal entry. Everyone knows about Celshading. Cheap Anime games use it to try and make 3D look 2D and often fail spectacularly, although its still a nifty effect.

After studying The Secret of NIMH, I've noticed several particular aspects of the movie that could feasibly be recreated in a 3D graphics engine in such a way that it would be ***indistinguishable from the real thing***, provided it did not break certain constraints.

**Shading**: Cel-shading *is not used* all the time. Modern anime-inspired animation often likes to cel-shade everything, but if you look at many classic films, it's actually only used in instances of high contrast, and more particularly its very unevenly done, even if its a relatively stable light source. In a 3D context this would mean flatshading almost all the dynamic models and implementing a very special lighting system that would only trigger with certain "tagged" lights (there is no possible way to dynamically figure this out in a manner consistent with traditional animation). In contrast to this, the backgrounds are extremely detailed paintings, which often end up being lit with either celshading or an interesting gradient effect.

A separate interpretative shader for the background could be adopted (along with a special case for lighting if necessary) to create the painting effect, but the most striking feature about all backgrounds in all 2D animations is the fact that they are always static. That is to say, if the character is "walking around a table," the background is really just a long, looping single picture. This is an effect that is exceedingly difficult to duplicate in 3D because it is very much not 3D. One way to get around this problem is to take a different approach, and that is to take advantage of the fact that in 99% of circumstances in a 2D animation, a given camera angle will stay on a flat 2D plane, even if the characters are supposedly moving around in 3 dimensions. Because of this, we can orient our camera for the beginning of a scene, and then *unwrap* the entire background's 3D model into a panoramic representation. If done correctly, it would allow an artist to build a 3D model and then orient the camera and figure out how the panning in a 2D context would be done from that perspective.

**Lines**: The lineart in a traditional 2D animation turns out to be one of the most important aspects, which is to be expected. However an unexpected detail that most of us miss when watching an animated film is the fact that those black lines are actually very, *very* thin. Forget the thick black lines you see them attempting to do in those idiotic 3D hackups, all of the lines in traditional animation are thin, and **the exact same width**. Except for eyebrows, they are all *the same width*. 

Hence, getting the lines right is going to be a make-or-break part of a 3D interpretative shader. This is usually done via an edge shader by doing a per-pixel depth check. In theory, this would also allow us to make lines that are relatively closer to the viewer thicker, and ones farther away, thinner, but it turns out that this isn't actually needed if we're mimicking traditional 2D animation (this is probably because doing so would be a total bitch to animate). I'm going to come back to this later so keep it in mind.

Getting lines right is hard for humans in the first place, and often requires meticulous design of a 3D model, but its even more ridiculous for furry mice. Hair in particular requires a static 3D model or it goes all over the place. The requirements for fur, however, are less strict, and often it looks completely wrong if you attempt to model it. The trick is to get the shader to do what a human does when examining where to put tufts of fur. Steep angles, joints, and 1-3 possible curls on longer stretches, equidistant apart with a slight deviation of approximately 10% of the length of the stretch. This is influenced by how "furry" a given section is; for example, Mrs. Brisby has a particularly furry chin and thus we end up with a large tuft there. what gets more ridiculous, however, is that if your going to get this to line up with the lines is that you have to model all these tufts of fur on the model itself so that the edge shader can properly line it. This can be done either with a displacement map or with a geometry shader.

**Realtime**: The possibility of a geometry shader opens up the stunning possibility of doing this **in realtime**. Because this algorithm translates a detailed 3D scene into a traditional 2D animation, it would suddenly become possible to do what was previously impossible - Design a 2D game that is *dynamic*, able to generate an entire, completely random forest, and then figure out how to render it as a background that looks painted and is unwrapped into a 2D panorama. The character that you control could look hand drawn and yet respond to literally anything using procedural animation. You could explore any angle in any way possible and the game could still figure out how its supposed to look.

**Future work**: This technique allows an animation to do things that were not previously possible, such as animating lines that take into account their distance from the viewer, as well as characters being able to interact with background elements without the background elements having to be cel-shaded. Even weirder possibilities include seamless integration of realistic CGI characters with cel-shaded ones, and putting cel-shaded characters in a realistic CGI environment, a la Roger Rabbit except there would be no combining footage - it'd all just work in a single environment and the 2D characters would be able to perfectly respond to their surroundings.

So yeah that's my essay for the week.
