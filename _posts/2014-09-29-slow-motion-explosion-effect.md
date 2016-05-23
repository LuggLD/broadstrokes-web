---
id: 43
title: Slow Motion Explosion Effect
date: 2014-09-29T12:09:05+00:00
author: Jan
layout: single
guid: http://www.broad-strokes.com/?p=43
permalink: /2014-09/slow-motion-explosion-effect/
categories:
  - Dev Blog
  - Tutorials
tags:
  - blackbody
  - effects
  - procedural
  - tessellation
  - tutorial
  - unrealengine4
---
Inspired by Epic&#8217;s Unreal Engine 4 <a href="https://www.youtube.com/watch?v=1dVAYmbIdfU" target="_blank">&#8220;Showdown&#8221; demo</a> shown at Oculus Connect, I thought I would try and recreate the slowmo explosion effect they talked about in <a href="https://de45xmedrsdbp.cloudfront.net/Resources/files/UE4-Integration-and-Demos_OC-100270768.pptx" target="_blank">their presentation</a>. Here&#8217;s what I came up with:

<div class="vine-embed">
</div>

This effect uses a simple sphere mesh as base, using a heightmap generated from a noise function for tesselation, and then sets each pixel&#8217;s heat based on their distance from the center, using a BlackBody function to get the appropriate color. There&#8217;s probably a more elegant or efficient way, but after some trial and error I arrived at this material setup:

[<img class="aligncenter wp-image-46 size-full" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material.jpg" alt="ue4-exp-material" width="1110" height="426" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material.jpg 1110w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material-300x115.jpg 300w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material-1024x393.jpg 1024w" sizes="(max-width: 1110px) 100vw, 1110px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material.jpg)

Here are the relevant properties of the noise function and the material, respectively:

[<img class="aligncenter wp-image-48 size-full" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-noiseprops.jpg" alt="ue4-exp-noiseprops" width="330" height="215" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-noiseprops.jpg 330w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-noiseprops-300x195.jpg 300w" sizes="(max-width: 330px) 100vw, 330px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-noiseprops.jpg)

[<img class="aligncenter wp-image-47 size-full" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-matprops.jpg" alt="ue4-exp-matprops" width="328" height="359" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-matprops.jpg 328w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-matprops-274x300.jpg 274w" sizes="(max-width: 328px) 100vw, 328px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-matprops.jpg)

Since the noise we use to generate the shape is based on absolute world location, all that had to be done to animate it was to move the location of the base polygons. To achieve that, I made a quick and dirty blueprint to scale up the sphere over time and make it move upwards. I found out that to keep the coloring more or less consistent, I also had to scale the BlackBody Bias and BlackBody Temp Max parameters accordingly.

[<img class="aligncenter wp-image-45 size-full" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp2.jpg" alt="ue4-exp-bp2" width="847" height="791" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp2.jpg 847w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp2-300x280.jpg 300w" sizes="(max-width: 847px) 100vw, 847px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp2.jpg) [<img class="aligncenter wp-image-44 size-full" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp1.jpg" alt="ue4-exp-bp1" width="631" height="231" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp1.jpg 631w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp1-300x110.jpg 300w" sizes="(max-width: 631px) 100vw, 631px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-bp1.jpg)

My default values for the parameter adjustments are to have the Cooling Per Sec set at slightly more than half of the Heating Per Sec value, which makes the material turn from a hot fireball into a cooler cloud of black smoke over time. Another thing you can do to make this a little more realistic is to add some upwards motion to the noise. It&#8217;s as simple as replacing the Absolute World Location input to the Noise function with something like this:

[<img class="aligncenter wp-image-57 size-full" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material-pan.jpg" alt="ue4-exp-material-pan" width="651" height="443" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material-pan.jpg 651w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material-pan-300x204.jpg 300w" sizes="(max-width: 651px) 100vw, 651px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2014/09/ue4-exp-material-pan.jpg)

Which changes the result to something much more like an explosion, and less like a fireball in space:

<div class="vine-embed">
</div>

And there you have it! Enjoy 🙂 Feel free to poke me <a href="https://twitter.com/JKashaar" target="_blank">on Twitter</a> if you have any questions!