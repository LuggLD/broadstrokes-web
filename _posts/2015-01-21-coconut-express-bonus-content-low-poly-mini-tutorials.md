---
id: 188
title: 'Coconut Express Bonus Content: Low-poly Mini-Tutorials!'
date: 2015-01-21T22:01:29+00:00
author: Jan
layout: single
guid: http://www.broad-strokes.com/?p=188
permalink: /2015-01/coconut-express-bonus-content-low-poly-mini-tutorials/
categories:
  - Dev Blog
  - Tutorials
tags:
  - blueprint
  - effects
  - gamejam
  - lowpoly
  - menus
  - tutorial
  - unrealengine4
  - water
---
_This post is an addendum to my <a href="https://www.unrealengine.com/blog/smooth-sailing-on-the-coconut-express" target="_blank">guest blog</a> on the official UnrealEngine <a href="https://www.unrealengine.com/blog/smooth-sailing-on-the-coconut-express" target="_blank">website</a>. Enjoy!_

### How to use flat-shading on a landscape material

This is pretty simple, actually! If it were a Static Mesh instead of a Landscape, you could simply import it without smoothing groups for flat shading, but (as far as I know) there is no way to directly tell a material to ignore mesh smoothing. But you can get around this by deriving the normals from mesh geometry using this setup:

<figure>
<img class=" wp-image-191 size-full aligncenter" title="" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-flatshading.jpg" alt="" width="812" height="283" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-flatshading.jpg 812w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-flatshading-300x105.jpg 300w" sizes="(max-width: 812px) 100vw, 812px" /><figcaption>

For this to work, you also need to disable “Tangent Space Normals” in the material properties.</figcaption></figure>

To explain why this works, we&#8217;ll have to get a little technical for a moment: The DDX and DDY functions return vectors pointing in the direction of the U and V axes of a pixel’s UV Map. Since those two vectors effectively describe a plane perfectly aligned with the mesh’s surface, their cross product will return a vector perpendicular to this plane – that polygon’s normal vector. Voilà, flat shaded materials!

### How to make procedurally animated low-poly water

<img class=" size-large wp-image-196 aligncenter" title="" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-ocean-1024x524.jpg" alt="coconuts-ocean" width="640" height="328" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-ocean-1024x524.jpg 1024w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-ocean-300x154.jpg 300w" sizes="(max-width: 640px) 100vw, 640px" />

The ocean in The Coconut Express uses a simple but effective shader applied to a rotating circular mesh. The mesh is a (slightly bent) circle with a lot of subdivisions, of around 8000 polys. This high polycount is needed to provide enough detail when tessellation is applied, as you’ll see in a moment. This is the material setup for the ocean blueprint:

<img class=" size-large wp-image-198 aligncenter" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanmaterial-1024x637.jpg" alt="coconuts-oceanmaterial" width="640" height="398" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanmaterial-1024x637.jpg 1024w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanmaterial-300x187.jpg 300w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanmaterial.jpg 1335w" sizes="(max-width: 640px) 100vw, 640px" />

As you can see, it uses the same flat-shading technique as described for the landscape above, so let’s pick apart the other parts! First off, the tessellation. This is pretty standard fare. If you don’t know what it is and how it works, check out [this other tutorial](http://www.broad-strokes.com/2014-10/a-quick-tessellation-trick/) I wrote about it a little while ago. The biggest difference is that here, the “heightmap” used for tessellation is not actually a texture, but is procedurally generated from the output of the noise part of this material. So let’s look at that next!

The Noise node is the core of this section. If you click on it, you can adjust a lot of properties, such as algorithm type, minimum and maximum output, quality, etc. In this case, I’ve used Perlin noise with 2 levels of turbulence, a min output of 0 and a max output of 1. That limits the output to the 0.0-1.0 scale used for linear color channels in UE4’s material system.

Now, what about the rest of the noise section? The Noise node’s “position” input can be filled by any three-dimensional vector, but it’s meant to go with positions in world space. You could connect an Absolute World Position node to it and get servicable noise! So what do the rest of these nodes do? First of all, we wanted our water to have animated waves. To achieve that, I used a Time input combined with a factor for animation speed, which works to move the noise’s position input upward over time, creating a nice, subtly bubbling animation.

The Position Transform node (shown as “World to Local”) is used in order to allow us to rotate the mesh, and have its noise function rotate with it. Without it, the noise would stay in place even as the mesh it is applied to moves, which looks really weird. The mesh rotation uses this very simple blueprint setup:

[<img class=" size-full wp-image-197 aligncenter" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanbp.jpg" alt="coconuts-oceanbp" width="663" height="287" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanbp.jpg 663w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanbp-300x130.jpg 300w" sizes="(max-width: 663px) 100vw, 663px" />](http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-oceanbp.jpg)

With the rotation rate set to a yaw value of 1 with the rest zeroed out, that combines with the noise material to create a nice, slow panning movement of the waves:
  


### Blueprint: Handling Gameplay Zones

As our core gameplay idea, we wanted the player to have to manage their swallows’ stamina, with different terrain areas having different effects on their speed and rate of energy consumption/recharge. Mountaineous terrain would cost much more energy to cross but would speed you up (because of stronger winds, you see! Look, we don&#8217;t really know anything about birds). Towns and forests on the other hand would slow you down but recharge your energy, with forests also acting as shelter from the evil falcons.

<img class=" size-large wp-image-200 aligncenter" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-zones-1024x555.jpg" alt="coconuts-zones" width="640" height="347" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-zones-1024x555.jpg 1024w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-zones-300x163.jpg 300w" sizes="(max-width: 640px) 100vw, 640px" />

To realize this on a tools level, I made a generic “Zone” blueprint. These Zones can overlap each other, can be assigned a priority, and can be rotated and resized as desired by the level designer. When the player character overlaps with a zone, this zone gets registered in an array in the character BP. On end overlap, it is removed again.

The logic to determine the dominant zone is handled entirely within the character itself. Every time a new element is added to or removed from the “Affecting Zones” array in the character, a check is run over the entire array to pick out the zone with the most dominant type and highest priority. That allowed us to handle the swallows’ energy consumption and speed through state changes based on the single dominant zone the player was currently in.

### Blueprint: Custom 3D menus with meshes

<img class=" size-full wp-image-190 alignright" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menutransition.gif" alt="coconuts-menutransition" width="400" height="227" />

I’ve had some inquiries about how we made the main menu for this game, so here’s a quick overview for everyone who wants to make their own 3D menus. Ours is a composite blueprint complete with a camera component, all menu items, the selector mesh, and logic for handling input events.

To make this work, the level blueprint enables cinematic mode on Begin Play, and switches the player controller’s view target to the Main Menu blueprint with a gradual blend, to create a nice camera transition.

Since events and functions in level blueprints cannot be called directly from class blueprints, our Menu BP contains an Event Dispatcher. The red line you see in the next picture connects to a “Bind Event to” node for that dispatcher, so that when we call the dispatcher after a round is completed, it calls the “SwitchToMenu” event in the level blueprint and the player’s view is reset to the menu camera.

<img class=" size-large wp-image-195 aligncenter" title="" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuswitch-1024x330.jpg" alt="coconuts-menuswitch" width="640" height="206" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuswitch-1024x330.jpg 1024w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuswitch-300x97.jpg 300w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuswitch.jpg 1436w" sizes="(max-width: 640px) 100vw, 640px" />

Input handling is pretty straightforward. The InputActions for up and down change an Integer variable indicating the currently selected item, and the selector mesh’s position is updated accordingly.

<a href="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-inputselector.jpg"><img class=" size-large wp-image-192 aligncenter" title="" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-inputselector-1024x504.jpg" alt="coconuts-inputselector" width="640" height="315" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-inputselector-1024x504.jpg 1024w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-inputselector-300x148.jpg 300w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-inputselector.jpg 1490w" sizes="(max-width: 640px) 100vw, 640px" /></a>

When the player hits Enter or the confirmation button on their controller, an action is executed based on the currently selected menu item:

<a href="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuconfirm.jpg"><img class=" size-large wp-image-194 aligncenter" title="" src="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuconfirm-1024x535.jpg" alt="coconuts-menuconfirm" width="640" height="334" srcset="http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuconfirm-1024x535.jpg 1024w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuconfirm-300x157.jpg 300w, http://www.broad-strokes.com/wordpress/wp-content/uploads/2015/01/coconuts-menuconfirm.jpg 1476w" sizes="(max-width: 640px) 100vw, 640px" /></a>

And that’s the gist of it. Don’t forget to enable/disable input on the Menu actor as you switch to/away from it, and you’re golden!

&nbsp;

If you have any questions or comments, or just want to connect, just leave a comment or poke me <a href="https://twitter.com/JKashaar" target="_blank">on Twitter</a>!

You can also download and get more info about [The Coconut Express](http://www.broad-strokes.com/games/the-coconut-express/ "The Coconut Express") here!