---
id: 453
title: Make Floating Rocks with the Power of Math™! (Part 1)
date: 2016-05-08T23:27:25+00:00
author: Jan
layout: single
guid: http://www.broad-strokes.com/?p=453
permalink: /2016-05/floating-rocks-the-power-of-math-part-1/
header:
  image: wp-content/uploads/2016/05/og-image.jpg
  teaser: wp-content/uploads/2016/05/og-image.jpg
enclosure:
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelDemo.webm
        95890
        video/webm
        
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured.webm
        1600715
        video/webm
        
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.webm
        282902
        video/webm
        
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured.mp4
        2035149
        video/mp4
        
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.mp4
        450566
        video/mp4
        
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelDemo.mp4
        825232
        video/mp4
        
categories:
  - Tutorials
tags:
  - blueprints
  - math
  - tutorial
  - unrealengine4
---
So, you thought to yourself, &#8220;Man, wouldn&#8217;t it be cool if I could add some floating rocks to my level?&#8221; Well, I heard those thoughts you were thinking, so here&#8217;s a quick tutorial for how to do that kind of stuff pretty simply with blueprints!

This tutorial assumes you know at least the basics of how to use UE4 Blueprints &#8211; I&#8217;m more concerned with showing you the why&#8217;s and how&#8217;s of the math, and the thinking behind these things. If you have any questions, feel free to use any of the social media methods linked above, or just post a comment!

## The thing we&#8217;re going to build

I just built this tool here yesterday for a scene my friend <a href="https://twitter.com/OBLIQUE_JLR" target="_blank">Jon Riley</a> is making (all meshes and textures used here are made by him!), and we&#8217;ll use it as an example to show some things you can do to make those stones float nicely:

<iframe width="640" height="360" src="https://www.youtube.com/embed/oih6NwVrOOM" frameborder="0" allowfullscreen></iframe>

It&#8217;s pretty simple, really! Let&#8217;s look at the big stone at the top first. As you can see below, it does some very slow undulating rotations and a bit of bobbing.

Normal speed:

<video controls="controls" width="500" height="700"><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured.webm" type="video/webm" /><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured.mp4" type="video/mp4" />Your browser does not support the video tag.</video>

Here&#8217;s the same thing at 8x speed, so you can see it better:

<video controls="controls" width="500" height="700"><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.webm" type="video/webm" /><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.mp4" type="video/mp4" />Your browser does not support the video tag.</video>
  
Let&#8217;s pick it apart then, shall we? 🙂 As you can see in the sped-up version of the video, the rotation of that rock is basically a **swivel motion**, combined with some **vertical bobbing**. The swiveling is more interesting, so let&#8217;s tackle that first!

## Swivel those rocks!

Swiveling is defined as &#8220;turning around a point&#8221;, though in this case it makes more sense to think of it as rotating around an **axis**. Here&#8217;s a visual illustration of the math we&#8217;re going to use:

<video controls="controls" width="300" height="300"><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelDemo.webm" type="video/webm" /><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelDemo.mp4" type="video/mp4" />Your browser does not support the video tag.</video>

To get started with that, let&#8217;s first create the swivel vector in our blueprint&#8217;s construction script. If you paid attention in the video at the start of this post, you&#8217;ll have noticed that the user control for this is not a vector input, but just a float to specify the swivel angle. To get a swivel vector from that, we have to do our first bit of math!

[<img class="alignnone wp-image-499 size-full" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelAxis2.png" width="1207" height="538" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelAxis2.png 1207w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelAxis2-300x134.png 300w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelAxis2-768x342.png 768w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelAxis2-1024x456.png 1024w" sizes="(max-width: 1207px) 100vw, 1207px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelAxis2.png)

I added some squiggly notes so that you can hopefully better understand what does what. The Swivel Cone X/Y float variables are editable variables that your level designer can use to tweak how exaggerated the swiveling should be. Swivel Axis X/Y are our blue vector above, for the X and Y axis respectively. We&#8217;ll rotate those around the local space X/Y axes on every frame by a little bit, in order to get that nice undulating swivel motion.

## It&#8217;s no Sine to enjoy maths!

Now, I assume that you already know at least the basics about what sine waves are, but just in case, here&#8217;s a quick refresher in the form of an image I shamelessly stole from a Google image search:

[<img class="alignnone size-full wp-image-459" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Wave-Terminology.gif" alt="Wave-Terminology" width="357" height="229" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Wave-Terminology.gif)

The other input (besides the swivel cone angle) that level designers get with this tool is the **rotation period**. As the diagram above so helpfully illustrates, that means: the time it takes to perform one full cycle. I decided to use period, because it&#8217;s the most human-relatable number, even if it makes the math we&#8217;re doing a little bit more complex. Always plan for user-friendliness! The less you have to explain how the tools you build work, the more time you can spend actually building your tools 🙂

Here&#8217;s the math that creates the swivel motion:

[<img class="alignnone size-full wp-image-463" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress-1.png" alt="SwivelProgress" width="1199" height="735" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress-1.png 1199w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress-1-300x184.png 300w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress-1-768x471.png 768w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress-1-1024x628.png 1024w" sizes="(max-width: 1199px) 100vw, 1199px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress-1.png)

Alright now, don&#8217;t get intimidated! This is actually quite simple. Let&#8217;s pick it apart bit by bit!

## On Ticks and the illusion of smooth movement

First of all, what&#8217;s a &#8220;Tick&#8221;? Does it bite? No, Tick is an **Event** that gets called by the engine on every single frame that is rendered. It&#8217;s a common misconception that somehow things in a game are happening smoothly and gradually. The truth is that things happen in steps. Very small steps, yes, but still. For example, if your game renders at 60 FPS, that means there are 60 steps to everything, every second. Game engines generally work like this: First, all the logic is executed. For example, every moving object has its position and rotation updated by whatever drives it, projectiles fly forward, player input is received and handled, and so on. Next, the engine takes all that information and gives the relevant parts of it to the renderer, so that it can produce the image that you see on screen. And then it goes back to the first step, forever repeating until someone presses Alt+F4.

So, games really only give the illusion of things moving smoothly. Just like films! Although&#8230; am I the only one thinking that 24FPS for films is really just not enough? I hope the glorious future of VR movies changes that habit&#8230;

ANYWAY. Back to the math. Let&#8217;s just look at one of those parts in isolation.

[<img class="alignnone size-full wp-image-464" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress2-1.png" alt="SwivelProgress2" width="878" height="219" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress2-1.png 878w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress2-1-300x75.png 300w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress2-1-768x192.png 768w" sizes="(max-width: 878px) 100vw, 878px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress2-1.png)

&#8220;**Delta Seconds**&#8220;, by the way, is the real-world time that has elapsed between the last Tick and this one. It&#8217;s great if you want things to move at the same speed, regardless of framerate! Which you should always want. Here, the first bit of math that we do is to multiply delta seconds with 360 so that one second equals 360° of rotation. Then we divide that by the Swivel Period, which, you guessed it, is the period (see that sine wave terminology graph above) for one full cycle. The result: we get the amount of degrees that something should rotate in this tick, if it has the rotation period we specified!

The next bit is pretty common too: we take that result, and add it to our Current Swivel variable. We want this value to **continually count upwards**, so to do that, we have to take the rotation of _this_ frame, and add it to our current rotation. That way it continually counts upwards, and in a framerate-independent way! Neat, huh?

Now you might wonder what that node with the percentage sign is for. That&#8217;s a &#8220;**modulo**&#8221; node. If you remember your elementary school math, when you had to do divisions by hand, whenever a dividend wasn&#8217;t a full multiple of the divisor, you always got a result, and a remainder. Modulo is basically that: it divides input A by input B, but instead of returning the result, it returns the remainder. So, for example&#8230; 13 modulo 10 equals 3.

In this case, we&#8217;re using it so that the rotation &#8220;loops&#8221; when it reaches 360°. This won&#8217;t be noticable in game, but it&#8217;s a good practice because if numbers get too high, at some point you might run into float precision errors. Let&#8217;s not get into that right now though!

Onward:

## Doing something with all this math stuff we so bravely calculated

[<img class="alignnone size-full wp-image-466" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3.png" alt="SwivelProgress3" width="587" height="272" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3.png 587w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3-300x139.png 300w" sizes="(max-width: 587px) 100vw, 587px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3.png)

Doesn&#8217;t this look familiar to you? If not, check above! We already used the **Rotate Vector Around Axis** function in the Construction Script. Here, instead of using the straight-up coordinate axis as the In Vector, we use **the rotated vector** that we created from the cone angle earlier in the Construction Script, and rotate _that_ around the local coordinate axis. That makes the vector swivel exactly like the one in the example with the two arrows above!

The next thing you might be wondering about: &#8220;Make Rot from XY.&#8221; UE4 has a whole number of &#8220;Make Rot from &#8230;&#8221; functions, and to understand what they do, you need to understand a little bit about Rotators and coordinate systems. If you already know all this stuff, just skip on to the next big section! 🙂

## A few words about vectors, rotators and transforms

Let&#8217;s talk about **Rotators** first. In Blueprint, a Rotator (colored light blue, like the wire coming out of the Make Rot node in the screenshot up there) is composed of three components: **Roll**, **Pitch**, and **Yaw**. Roll is rotation around the X axis, pitch is rotation around the Y axis, and yaw is rotation around the Z axis.

By convention, UE4 considers the X axis as the forward direction, Y as right, and Z as up. Rotators are simply the amount of degrees that something needs to be rotated from the default rotation (facing forward down the X axis) on each axis, in order to reach the desired orientation.

&#8220;Why not just use vectors?&#8221;, you might wonder. There&#8217;s a simple explanation &#8211; allow me to illustrate! You all know this guy, right?

[<img class="alignnone size-full wp-image-471" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot1.png" alt="CharacterRot1" width="177" height="344" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot1.png 177w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot1-154x300.png 154w" sizes="(max-width: 177px) 100vw, 177px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot1.png)

Think of that little blue arrow there as a vector that indicates the character&#8217;s forward direction. But is that enough information to exactly determine its rotation? Sadly not. Observe:

[<img class="alignnone size-full wp-image-472" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot2.png" alt="CharacterRot2" width="170" height="333" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot2.png 170w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot2-153x300.png 153w" sizes="(max-width: 170px) 100vw, 170px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/CharacterRot2.png)

The character&#8217;s forward arrow still points exactly the same way, but we can all see that that&#8217;s not how it&#8217;s supposed to be rotated, right? Vectors are great for indicating locations or directions, but if you need more than that, it&#8217;s best to use a rotator instead.

This is why each and every actor and object that exists in the game world has what&#8217;s called a &#8220;**Transform**&#8220;. Transforms are totally neat and useful! In code, they&#8217;re usually represented by a 4&#215;4 Matrix, which&#8230; stop, come back, don&#8217;t run away! Math just wants to be your friend! And don&#8217;t worry, in Blueprints it&#8217;s _a lot_ simpler: Transforms are represented by **Location**, **Rotation**, and **Scale**. The neat thing about transforms is that you can use them to represent any _transformation_ (a-ha!) from one spatial configuration to any other in one single operation, and you can even combine multiple transforms into a single one!

As a side-note, you may have already encountered mentions of &#8220;Relative&#8221; and &#8220;World&#8221; space. If it&#8217;s not obvious what that means, &#8220;relative&#8221; space is simply a coordinate system that is transformed so that it aligns with the Transform of whatever object its relative to &#8211; meaning that the relative X axis is the object&#8217;s forward axis, the relative Z axis is the object&#8217;s up axis, and so on.

## Make Rot from what?

Alright, now that you know all that or have refreshed your knowledge, what do the Make Rot from &#8230; nodes do? Simply put, they let you specify one or two **axis vectors**, and create a Rotator based on that. Make Rot from X, when given an input vector, treats that vector as the Rotator&#8217;s target coordinate system&#8217;s forward vector. As we&#8217;ve established above though, one vector alone does not give you reliable results! While UE is smart and will try to guess what you mean (in the case of Make Rot from X, it assumes that the standard up vector &#8211; (0,0,1) &#8211; is your desired up vector, for example), you can get more precise control by supplying two vectors. Make Rot from XZ for example creates a Rotator that points forward down the supplied X input vector, and then rolls it (remember: roll is rotation around the X axis) so that the output Rotator&#8217;s up vector is as close to the Z input vector as possible. Make Rot from ZX does the opposite: it creates a Rotator where the Z input is guaranteed up, and then it&#8217;s rotated around that axis so that forward is in the direction of X.

Side-note: that means that the two input vectors don&#8217;t have to be perpendicular &#8211; the first one is taken as the guide, and the second one is approximated as closely as possible. Except it&#8217;s not approximation at all, but exact math &#8211; it&#8217;s all a bunch of trigonometry under the hood, but really, you don&#8217;t have to worry about it too much.

So, what are we doing here then? As a refresher, and so you don&#8217;t have to scroll up, this is what we were talking about:

[<img class="alignnone size-full wp-image-466" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3.png" alt="SwivelProgress3" width="587" height="272" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3.png 587w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3-300x139.png 300w" sizes="(max-width: 587px) 100vw, 587px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SwivelProgress3.png)

As we&#8217;ve already established above, the outputs of those two Rotate Vector Around Axis nodes &#8211; over time, i.e. over the duration of multiple ticks &#8211; describe two vectors that perform **a swiveling motion in a cone**, of whatever angle we&#8217;ve let our level designers define, around the X and Y axis respectively. Now we simply take those two axes, and make a Rotator out of them.

And since the speed and angle of the two swivel motions are different from each other, that gives us a nice, natural-looking and seemingly non-looping undulating rotation!

So now, all that&#8217;s left is connecting that Rotator output into a Set Relative Rotation function for the top rock mesh component, hit the big Play or Simulate button, and we can see it doing its thing! Neat, huh? 🙂

&nbsp;

Well, that&#8217;s it for this first part, folks! Part 2 will deal with the big rock&#8217;s bobbing movement, and part 3 will explain how to make those neat little orbiting stones.

I hope you found this tutorial useful! If you have a moment, please feel free leave me some feedback in the comments &#8211; not sure if I went too in-depth here, or not in-depth enough&#8230; but I hope I managed to strike a good balance. If this tutorial taught you anything, you could also do me a solid and click one of those share buttons below!

Till next time!

Update: Part 2 is available! Read it [here](/2016-05/make-floating-rocks-with-the-power-of-math-part-2/).