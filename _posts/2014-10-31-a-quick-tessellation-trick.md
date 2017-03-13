---
id: 109
title: A quick tessellation trick
date: 2014-10-31T19:42:02+00:00
author: Jan
layout: single
guid: https://www.broad-strokes.com/?p=109
permalink: /2014-10/a-quick-tessellation-trick/
categories:
  - Dev Blog
  - Tutorials
tags:
  - materials
  - tessellation
  - tutorial
  - unrealengine4
---
Let&#8217;s talk a little bit about tessellation in Unreal Engine 4. I&#8217;ve been experimenting with this recently because the usual techniques involving normal maps to add surface detail to an object simply don&#8217;t hold up well in virtual reality, and my development is focused on VR. What tessellation does is to subdivide a 3D model&#8217;s geometry to add more detail, and it does this entirely on the GPU. Consider this standard template cube model that comes with the engine:

[<img class="aligncenter wp-image-111 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellated-cube_static.jpg" alt="" width="521" height="453" srcset="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellated-cube_static.jpg 521w, https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellated-cube_static-300x261.jpg 300w" sizes="(max-width: 521px) 100vw, 521px" />](https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellated-cube_static.jpg)

With tessellation enabled, those faces will be subdivided dynamically, based on the tessellation multiplier, like so:

[<img class="aligncenter wp-image-110 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellated-cube.gif" alt="Tessellated Cube" width="694" height="603" />](https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellated-cube.gif)

Now, the most useful aspect of tessellation is that you can use that dynamically created geometry and offset it from the base mesh, creating not just the illusion of detail as you would normally do with normal maps, but add actual geometric detail. This is done through the use of height maps, monochrome textures that tell the renderer which parts of the texture are elevated and which ones are not. The tradeoff for this is performance &#8211; the added tessellation detail comes at a rendering cost, and you should always take care to check whether a given mesh really needs tessellation or not &#8211; so don&#8217;t simply use it everywhere. Older graphics cards without full DirectX 11 support might not even support it, so take that into account as well.

So, with that disclaimer out of the way, let&#8217;s set up a tessellation material! It&#8217;s pretty straightforward: the first thing we have to do is to enable tessellation in the material properties, by setting the D3D11 Tessellation mode to either Flat Tessellation or PN Triangles. The difference is <a href="https://docs.unrealengine.com/latest/INT/Resources/ContentExamples/MaterialProperties/1_8/index.html" target="_blank">explained here</a>. That will enable the World Displacement and Tessellation Multiplier output pins in the material, and allow you to connect things to it. Here&#8217;s a very basic tessellation material setup:

[<img class="aligncenter wp-image-115 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup.jpg" alt="tessellation-materialsetup" width="410" height="430" srcset="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup.jpg 410w, https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup-286x300.jpg 286w" sizes="(max-width: 410px) 100vw, 410px" />](https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup.jpg)

[<img class="aligncenter wp-image-118 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup2.jpg" alt="tessellation-materialsetup2" width="743" height="632" srcset="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup2.jpg 743w, https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup2-300x255.jpg 300w" sizes="(max-width: 743px) 100vw, 743px" />](https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-materialsetup2.jpg)

The way this works is that for every pixel this shader renders, it takes the mesh&#8217;s vertex normal which, in case you don&#8217;t know, is a normal vector pointing outwards from the surfaces of the mesh, linearly interpolated between mesh vertices according to smoothing groups and face orientation. It then uses a height map aligned to the mesh&#8217;s UV coordinates, multiplied by the Tessellation Height Factor, which basically just tells the shader how far in world units the surface should be displaced from their original position, and scales the normal vector by that amount.

Now, the problem with this simple approach is that it only displaces a surface outward, so the more you increase the Height Factor, the bigger your mesh&#8217;s silhouette gets.

[<img class="aligncenter wp-image-119 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-problem.gif" alt="tessellation-problem" width="365" height="485" />](https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-problem.gif)

As you can imagine, this complicates things. One of the problems with this setup is that since tessellation doesn&#8217;t apply to collision, even if you have complex per-poly collision enabled for your mesh, it&#8217;s possible for the player&#8217;s viewpoint to clip into the mesh if it is heavily tessellated.

Thankfully there is a simple workaround to adjust that, which only involves a tiny little bit of maths. What we did before simply adds a positive offset to the mesh&#8217;s surface. Instead, we should average it so that the displacement goes inwards and outwards in equal amounts. This is quite simple! Just replace this part of your material:

<figure id="attachment_121" style="width: 584px" class="wp-caption aligncenter">
<img class="wp-image-121 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-standardsetup1.jpg" alt="" width="584" height="316" srcset="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-standardsetup1.jpg 584w, https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-standardsetup1-300x162.jpg 300w" sizes="(max-width: 584px) 100vw, 584px" />
<figcaption class="wp-caption-text">Standard setup</figcaption></figure>

&#8230;with this:
<figure id="attachment_120" style="width: 760px" class="wp-caption aligncenter">
<img class="wp-image-120 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-adjustedsetup1.jpg" alt="" width="760" height="333" srcset="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-adjustedsetup1.jpg 760w, https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-adjustedsetup1-300x131.jpg 300w" sizes="(max-width: 760px) 100vw, 760px" />
<figcaption class="wp-caption-text">Adjusted setup</figcaption></figure>

In the example above, we add anything between 0-50 displacement to the mesh, depending on whether the height map is more black (=0) or more white (=1), which then gets multiplied by 50. To correct for this, in the second example we simply subtract 25 from the result (the &#8220;Tessellation Scale&#8221; parameter divided by 2), before multiplying it with the vertex normals. That means that now, the blacks in our heightmap are at -25, and the whites are at +25 offset from the mesh. Simple, right? 🙂

Here&#8217;s what the adjusted setup looks like compared to the unadjusted one. As you can see, the mesh is displaced both inwards and outwards:
<figure id="attachment_114" style="width: 561px" class="wp-caption aligncenter">
<img class="wp-image-114 size-full" src="https://www.broad-strokes.com/images/wp-content/uploads/2014/10/tessellation-example2_anim2.gif" alt="tessellation-example2_anim2" width="561" height="402" />
<figcaption class="wp-caption-text">Don&#8217;t get confused by the huge Tessellation Height Factors, that&#8217;s one huge mesh I&#8217;m using there.</figcaption></figure>

Alright, that&#8217;s it for this simple technique. Hope you learned something! Feel free to contact me on <a href="http://twitter.com/JKashaar" target="_blank">Twitter</a> or comment on this post if you have any questions or suggestions for other tutorials you would like to see!

&nbsp;
