---
id: 479
title: Make Floating Rocks with the Power Of Math™! (Part 2)
date: 2016-05-16T05:55:15+00:00
author: Jan
layout: single
guid: http://www.broad-strokes.com/?p=479
permalink: /2016-05/make-floating-rocks-with-the-power-of-math-part-2/
header:
  teaser: /images/features/sine-vertical.gif
  image: /images/wp-content/uploads/2016/05/og-image.jpg
enclosure:
  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.webm
        282902
        video/webm

  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.mp4
        450566
        video/mp4

  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-BobOnly.webm
        1487874
        video/webm

  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Vector-3D-Widgets.webm
        328034
        video/webm

  - |
    |
        http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Vector-3D-Widgets.mp4
        1167884
        video/mp4

categories:
  - Tutorials
tags:
  - blueprint
  - math
  - tutorial
  - unrealengine4
---
Welcome back, everyone! This is part 2 of my tutorial mini-series about how to make things move nicely using your awesome powers of math! ([See part 1 here](http://www.broad-strokes.com/2016-05/floating-rocks-the-power-of-math-part-1/))

As with part 1 of this series, I&#8217;m going to assume that you know at least the basics of Unreal Engine Blueprints already &#8211; if not, check out this official <a href="https://www.youtube.com/watch?v=cRhWc2kAhqI&list=PLZlv_N0_O1gbYMYfhhdzfW1tUV4jU0YxH&index=1" target="_blank">video tutorial series</a> by the Unreal Engine team, which should get you started! And since I only just set it up a few days ago: If you want me to keep you up-to-date with new tutorials I release, why don&#8217;t you subscribe to my [mailing list](http://www.broad-strokes.com/mailing-list/)? You&#8217;ll get notified when I post cool new tutorials or other interesting stuff!

Just to catch you up, I&#8217;m using this asset here to demonstrate a few simple ways of how to animate something using only blueprints and math:

<video controls="controls" width="500" height="700"><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.webm" type="video/webm" /><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-Faster.mp4" type="video/mp4" />Your browser does not support the video tag.</video>

(Shown at 8x speed)

In the [first part](http://www.broad-strokes.com/2016-05/floating-rocks-the-power-of-math-part-1/), I showed you how to create a nice swiveling motion. This second part is a bit simpler, and it will teach you how to improve that swiveling effect by adding some soft bobbing motion. Let&#8217;s turn off the swiveling for a moment, so you can see what the bobbing motion looks like all by itself:

<!--more-->

<video controls="controls" width="500" height="580"><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-BobOnly.webm" type="video/webm" /><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopRock-Textured-BobOnly.mp5" type="video/mp4" />Your browser does not support the video tag.</video>

Looks simple enough, right? And it also moves kind of unpredictably, which is exactly what we want. So, how do?

## Who&#8217;s this Bob guy?

Let&#8217;s define bobbing motion as smooth up and down movement, like what happens when you put an apple in a big bowl of water, and then punch it slightly, but not in a mean way. It&#8217;ll start bobbing up and down a bit! Bad example, probably, but I think you know what I mean, right?

So, how do we do this? We&#8217;ll need to use our awesome knowledge of sine functions that we either already had or acquired in [part 1 of this tutorial](http://www.broad-strokes.com/2016-05/make-floating-rocks-with-the-power-of-math-part-2/).

As always, illustrations help! Did you know that sine functions can be expressed like this?

<img class="alignnone size-full wp-image-554" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sine-circle.gif" alt="sine-circle" width="480" height="145" />

Let&#8217;s look at the right part of it first. On this graph, the X axis (the horizontal one), tracks the rotation angle. If you&#8217;re curious about the π stuff, don&#8217;t worry about that for now &#8211; that&#8217;s a different way to write angles, called &#8220;radians&#8221;, which you might have heard of. For ease of explanation, we&#8217;ll just use degrees &#8211; but if you&#8217;re curious:

You may remember from your math classes that the circumference of a circle is equal to its radius, multiplied with 2 π, or in equation form:

<p style="text-align: center;">
  c = 2 π r
</p>

Think of radians simply as the length of circumference, relative to the circle&#8217;s radius, you get at a circle section of that angle. As you can see in the animation above, at 360°, the radian value is exactly = 2 π, which is exactly the value for the circumference of a full circle. If we only take a half circle, i.e. a 180° angle, the radian value is half that = π.

Some functions in UE4 use radian input instead of degrees, and for some others (such as the trigonometric functions like sine, cosine, and tangent) there are two versions &#8211; one for degrees, and one for radians. Just something to be aware of!

So, what interests us here is purely the vertical movement on the Y-axis over time. Sine functions are a great way to create cyclical motion, and that&#8217;s exactly what we want. Take a look at how it looks if you animate a point using just a sine wave:

<img class="alignnone size-full wp-image-555" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sine-vertical.gif" alt="sine-vertical" width="500" height="236" />

The fun begins when you start to layer multiple sine waves with different periods, phases and amplitudes on top of one another, and that&#8217;s just what we&#8217;re going to do! Why, you wonder? Take a look:

## Layering multiple sine waves

To explain why layering multiple sine wave functions is cool and useful, let&#8217;s do a quick recap on the words we use to describe certain attributes of sine functions.

<img class="alignnone wp-image-550 size-full" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/SineTerms-1.gif" width="996" height="418" />

_**Amplitude**_ is the height of the wave&#8217;s peaks. Perhaps you&#8217;ve also heard this in the context of audio already &#8211; that&#8217;s because sound waves are waves too! And the higher the amplitude, the stronger the pressure in the air that carries the sound, and therefore, the louder the sound. Makes sense, right?

_**Period**_ is how long it takes for the function to do exactly one full cycle. Frequency, which you&#8217;ve probably heard more&#8230; frequently (I&#8217;m sorry) is used to describe the amount of full cycles per second. So basically, period and frequency are the inverse of each other &#8211; period is &#8220;seconds per cycle&#8221;, and frequency is &#8220;cycles per second.&#8221; When I build blueprint tools, I tend to use either, depending on which one gives users the easier to understand number. (Remember: Build easy-to-use tools! The rest of your team will thank you. Or just take it for granted, because you&#8217;ve spoiled them&#8230; oops.) For example, a frequency of 0.0167 is a lot less convenient for making small adjustments to than a period of 60 🙂

_**Phase**_, lastly, is  the horizontal offset. Unmodified, the sine function starts at 0/0, and then starts swinging upwards. With a phase of e.g. 0.5 as shown above, that 0/0 point is offset by 0.5 units to the right &#8211; or if we&#8217;re looking at it as a movement over time, it comes 0.5 seconds later.

Since we&#8217;re going to need it later, here&#8217;s how you express those attributes mathematically in a function:

<p style="text-align: center;">
  y = [amplitude] * sin ( ( [frequency] * x ) &#8211; [phase] )
</p>

<p style="text-align: left;">
  For ease of explanation, we&#8217;re going to ignore amplitude and phase for now, and just look at what combining multiple sine functions with different frequencies gets us! Let&#8217;s start with this sine function here:
</p>

<img class="alignnone wp-image-543 size-full" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sinelayer-wave1.gif" alt="sinelayer-wave1" width="647" height="148" />

As you can see, it has a period of about 2.3. Here&#8217;s another sine function:

<img class="alignnone size-full wp-image-544" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sinelayer-wave2.gif" alt="sinelayer-wave2" width="647" height="148" />

This one has a much higher period, which is why it looks so much flatter &#8211; its period is about 20.

Now I wonder what happens if we just add the two functions together&#8230;

<img class="alignnone size-full wp-image-545" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sinelayer-wave3.gif" alt="sinelayer-wave3" width="647" height="148" />

Can you see what&#8217;s happening? The function with the shorter period looks like it&#8217;s doing some oscillation of its own, in the same pattern as the other function! Let&#8217;s add another one:

<img class="alignnone size-full wp-image-546" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sinelayer-wave4.gif" alt="sinelayer-wave4" width="647" height="148" />

The result:

<img class="alignnone size-full wp-image-547" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/sinelayer-wave5.gif" alt="sinelayer-wave5" width="647" height="196" />

And now, we&#8217;re getting somewhere! Doesn&#8217;t that already look pretty irregular? There are of course still recognizable peaks and valleys, but it&#8217;s a lot harder to make out the overall repeating pattern now. Which is why we&#8217;re going to use three functions with configurable periods and amplitudes to give our rock a nice, irregular-seeming up-and-down movement.

Alright! Enough graphs, time to look at some Blueprint graphs instead!

## 3D Widgets: Drag & drop instead of type & hope

Before we go on &#8211; Do you want to know how to make these nice little handles that let you move stuff around without manually typing values by hand?

<video controls="controls" width="1164" height="630"><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Vector-3D-Widgets.webm" type="video/webm" /><source src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Vector-3D-Widgets.mp4" type="video/mp4" />Your browser does not support the video tag.</video>

Those are 3D Widgets, which are available for any Vector or Transform variable set as &#8220;editable.&#8221; Just select the variable in the My Blueprint list, and enable the option!

<img class="alignnone size-full wp-image-565" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopMeshLocation-VarDetails.png" alt="TopMeshLocation-VarDetails" width="346" height="541" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopMeshLocation-VarDetails.png 346w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopMeshLocation-VarDetails-192x300.png 192w" sizes="(max-width: 346px) 100vw, 346px" />

That will create one of those widgets for each such exposed variable. It even works for arrays! With vector widgets, you can only move them, but Transform widgets can be rotated and scaled as well. That can be quite useful when you&#8217;re building tools!

Note that I&#8217;ve also set the &#8220;Advanced Display&#8221; flag for this variable, so that it doesn&#8217;t show up in the Details panel unless users click on the little triangle button that expands the advanced options.

The second part of making parts of your Blueprint adjustable like that is to do something with the value of that 3D Widget&#8217;s vector variable, and in our case that means we have to set our TopMesh to the new default location in the Construction Script, and that&#8217;s all. Whenever a variable in a blueprint is changed, the construction script is executed again, so the location of the Top Mesh component is set to the new relative location the widget was just set to. Just like this:

<img class="alignnone size-full wp-image-566" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopMeshLocation-ConstructionScript.png" alt="TopMeshLocation-ConstructionScript" width="538" height="282" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopMeshLocation-ConstructionScript.png 538w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/TopMeshLocation-ConstructionScript-300x157.png 300w" sizes="(max-width: 538px) 100vw, 538px" />

## Bobbing, coming soon to a UE4 near you!

So how do we do this bobbing thing in UE4 then? Simple! We are going to re-use a lot of the things we learned in [part one](http://www.broad-strokes.com/2016-05/floating-rocks-the-power-of-math-part-1/) for this. First off, let me show you the part of our Blueprint that is responsible for the bobbing movement. This is the relevant part of the Tick event:<figure id="attachment_600" style="width: 792px" class="wp-caption alignnone">

[<img class="wp-image-600 size-large" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/BobbingFull-1-1024x270.png" alt="BobbingFull" width="792" height="209" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/BobbingFull-1-1024x270.png 1024w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/BobbingFull-1-300x79.png 300w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/BobbingFull-1-768x203.png 768w" sizes="(max-width: 792px) 100vw, 792px" />](http://www.broad-strokes.com/images/wp-content/uploads/2016/05/BobbingFull-1.png)<figcaption class="wp-caption-text">Click to enlarge!</figcaption></figure>

Don&#8217;t worry, it looks more complicated than it is, and we&#8217;re going to pick through it bit by bit. If you ever find yourself looking at a Blueprint and wondering what the hell is even going on, here&#8217;s a useful tip for you: most Blueprints can be best understood by working backwards! Start at the executable functions (the ones with the white &#8220;execution wires&#8221; going into and out of them), which tell you WHAT is being done. Most of the time, those have really helpful tooltips too, if you hover your mouse over them and their variables. Think of executable (also known as &#8220;impure&#8221;) functions as one line of code. They basically say: &#8220;And now, do this!&#8221; Any wires that go into these functions &#8216;only&#8217; specify HOW that function is supposed to do its job.

So then, let&#8217;s start with the &#8220;What&#8221; in this case, and work backwards through the &#8220;How&#8217;s&#8221;!

<img class="alignnone wp-image-562 size-full" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing3.png" width="679" height="418" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing3.png 679w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing3-300x185.png 300w" sizes="(max-width: 679px) 100vw, 679px" />

Fairly straightforward, I hope. We add two vectors, &#8220;Top Mesh Location&#8221; and one that we&#8217;re creating right there on the spot (with some more math that we&#8217;ll get to in a bit), and setting that as the new relative location of our floating rock mesh.

Next up, HOW do we calculate that vector which we add to our Top Mesh Location vector?

<img class="alignnone size-full wp-image-576" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing2a-1.png" alt="Bobbing2a" width="780" height="492" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing2a-1.png 780w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing2a-1-300x189.png 300w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing2a-1-768x484.png 768w" sizes="(max-width: 780px) 100vw, 780px" />

Look closely &#8211; it&#8217;s just three sine functions which we&#8217;re all adding together, just like in the graphical example above! Each one is also multiplied with its own amplitude variable, which just means that the output of the sine &#8211; which is in the -1 to 1 range &#8211; gets multiplied with it. So the amplitude variable is the distance in world units that each wave function makes the rock move upwards and downwards.

Lastly, here&#8217;s how we calculate the &#8220;Bobbing Cycle&#8221; variables:

<img class="alignnone size-large wp-image-577" src="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing1-1-1024x560.png" alt="Bobbing1" width="792" height="433" srcset="http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing1-1-1024x560.png 1024w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing1-1-300x164.png 300w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing1-1-768x420.png 768w, http://www.broad-strokes.com/images/wp-content/uploads/2016/05/Bobbing1-1.png 1216w" sizes="(max-width: 792px) 100vw, 792px" />

If you remember part 1, this is almost exactly [the same thing we did](http://www.broad-strokes.com/2016-05/floating-rocks-the-power-of-math-part-1/) for calculating the progress of the swivel motion! That&#8217;s because it is 🙂 Each &#8220;Bobbing Cycle&#8221; gets something added to it on every tick. We multiply Delta Time (Get World Delta Seconds has the same output as the Tick event&#8217;s Delta Time output, so if you want to get rid of some wires&#8230;) with 360, so that one second equals one full rotation, and then divide it by each bobbing cycle&#8217;s period to make sure that each cycle takes [period] seconds. That then gets added to the existing Bobbing Cycle variable, which loops back to 0 when it reaches 360. All stuff we&#8217;ve done before, at this point!

And if you combine this with the swivel motion we did [in part 1](http://www.broad-strokes.com/2016-05/floating-rocks-the-power-of-math-part-1/), you get a very nice, seemingly irregular movement of the stone, all without even touching any animation tools. It&#8217;s The Awesome Power Of Math™ at work for you!

&nbsp;

Well then! That&#8217;s it for now. Part 3, coming soon, will finally go into how to create those little orbiting rocky bits. It&#8217;s really very simple, but in the process you&#8217;ll learn a lot about how to customize things, and about random seeds which might give you a start on creating procedurally generated content in Unreal Engine 4! If you subscribe to my [mailing list](http://www.broad-strokes.com/mailing-list/), you don&#8217;t even have to check back here yourself to see if it&#8217;s out yet &#8211; just saying!
