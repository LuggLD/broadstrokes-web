---
id: 423
title: 'Over 9000 Swords &#8211; Unity Documentation'
date: 2015-11-23T19:46:02+00:00
author: Jan
layout: page
guid: https://www.broad-strokes.com/?page_id=423
---
## Getting Started

When you import the asset pack into unity, it should automatically unpack into the **‘Assets/ModularWeaponSystem’** folder.

Once this is done, you can create a new modular weapon game object by clicking **‘GameObject > 3d Object > Modular Weapon Object’**. Which will create a new ‘Modular Weapon’ object in your scene.

At first you will notice that the object appears empty, do not worry! All you need to do at this point is navigate to the **‘Generate’ button** in the Modular Weapon objects inspector and click it. This will generate a randomized weapon.

This is all you need to do if you just want a randomized weapon from our system. And clicking generate again will regenerate the existing weapon. If you want another Modular Weapon in your scene you can either repeat the above steps, or duplicate the existing one.

## How it works

When you import the asset pack into unity, an ‘Assets/ModularWeaponSystem/Resources’ subdirectory will be created, and in this directory there will be two files:

  * ModularWeaponContainer.asset
  * WeaponMaterialPresets.asset

These files are the core of our system and are essential for its operation. If either are missing you will be prompted to generate them again with a log error message. You can create these files by clicking **‘Assets > Create’** and choosing the appropriate file to generate.

Be aware, that if you delete either of these files and have to generate them again, you’ll need to add all of your presets again.

## Adding or Modifying presets

The Modular Weapon Container asset found in ‘Assets/ModularWeaponSystem/Resources’ is a container for all of the components used in the weapon generation system. It is comprised of a couple of nested lists which are used as follows.

  * **Weapon Type Name**: This defines what type of weapon is contained within this list. Each weapon type is ‘self contained’ in that the modular components within it won’t ever be generated for a different weapon type. As an example, if you were to add an ‘Axe’ weapon type to the system, none of the Axe subcomponents could be generated into a ‘Sword’ weapon type.
  * **Weapon Components**: These define the individual ‘blocks’ that the weapon is made up of. You will notice the default Sword type contains 4 components; ‘blade’, ‘guard’, ‘hilt’, ‘pommel’. Within each of these components you add all of the mesh components you want to be randomized. And you can check whether you want the component to be optional or not. Optional components have a weighted chance to be generated.
  * **Material Parameters**: This is where you define which shader parameters should be modified for your material. You’ll notice it contains a ‘Slot Type’ list comprising of ‘Main’, ‘Deco’ and ‘Energy’. Main describes both the primary and secondary colors, Deco describes the decorative color, and Energy defines any emissive components. All of these things are controlled by mask textures which will be detailed in the Materials section of this document.

## Materials

The Weapon Material Presets asset found in ‘Assets/ModularWeaponSystem/Resources’ is a container for all of the materials used by the Modular Weapon system. It contains a list of all of the material presets we provide, and it’s easy for you to add more! Each preset contains:

  * **Material Name**: The name of your material.
  * **Material Type**: Whether this affects the ‘Main’, ‘Deco’, or ‘Energy’ parameters.
  * **Material Unique Short Name**: The unique name for the material that is used in generation, while the Material Name can be anything you want, the short name MUST be unique.
  * **Color/Roughness/Metalness/Emissive** parameters define the look and feel of your material preset.

The material itself uses a bespoke shader written specifically for the weapon pack. The shader is listed under ‘Custom/Modular\_Weapon\_Swords’. The shader has exposed parameters for:

  * **Color**: Defines the color of the material, there is a color for primary/secondary/decoration/energy.
  * **Metallic**: Defines the metalness of the material, there is a metalness for primary/secondary/decoration/energy.
  * **Gloss Bias**: Defines the gloss bias of the material, there is a gloss bias for primary/secondary/decoration/energy. Gloss biases are added to the roughness texture.
  * **Primary/Secondary/Decoration/Energy texture**: This mask texture is used to control which of the above are used in the final material render and in what quantities. The Red channel lerps between Primary and Secondary, Green lerps to decoration, and Blue lerps to energy.
  * **Roughness/AmbientOcclusion/Dirt texture**: This mask texture is used to fine-tune some aspects of the material. Red is gloss/roughness, Green is ambient occlusion, Blue is dirt.
  * **Normalmap texture**.
  * **Invert Gloss**: If you’ve authored your content with roughness maps, check this to use Unity gloss instead.
  * **GGX to Blinn-Phong approx**: This applies a curve to your gloss output, it should only be used if you’ve generated your roughness/gloss maps in an application like e.g. Substance Designer or Painter, which use a GGX curve.
  * **Invert Normal Green**: This inverts the green/y channel of your normal map. Just in case you need to.

## Modular Weapon Inspector

At the beginning of this document we covered a very short ‘Getting Started’. But there are some other useful things to know about how the Modular Weapon system works!

  * **Weapon Code**: when you generate a weapon, a string is generated into this box. This string is unique to that weapon combination, and applies to both the mesh components and their materials. If you want to create a specific weapon combination, this is how you can specify it.
  * **Exclude Optional**: If this option is turned on, any optional mesh components will be discounted from the generation process.
  * **Optional Parts Weight**: this option determines how likely any optional parts are to be generated. 0 equals a 0% chance, 1 is a 100% chance.
  * **Random Types**: If you have multiple weapon types, this option allows you to decide whether to generate randomly between types, or only use the current type. If this option is off and the Weapon code string is empty, no weapon will generate.
  * **Random Parts**: Decides whether to randomize the mesh components of the current type, turn this off if you want to keep the current meshes but randomize the material presets.
  * **Random Materials**: Decides whether to randomize the material presets. Turn this off if you want to e.g. keep the current material combination but randomize the meshes.
  * **Randomize On Start**: This will make the weapon randomize (based on the above checks) when the game starts.

This system is designed to be as simple and easy to use for you as possible. We decided to go with a code string because it makes it easy for you to re-use weapons you like later on. If you make a weapon you think would look cool for every guardsman in your games army to use, copy that string into their weapons, and turn off Random Types, Parts, Materials, and On Start.

## Final Words

We designed this package to be easy to use, and easy for you to extend. If you wanted to add new weapon types, you just need to create the modular components and add them to the Modular Weapon Container as a new weapon type. Or if you wanted to clear out our existing material presets and create your own, you can do that too!

This document should hopefully give you a good starting point. But if you require further support, we have a dedicated Slack set up for real-time live support directly from the developers, and of course you can email us with any questions at <support@broad-strokes.com>!