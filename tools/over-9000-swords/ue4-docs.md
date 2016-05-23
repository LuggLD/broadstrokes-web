---
id: 426
title: 'Over 9000 Swords &#8211; UE4 Documentation'
date: 2015-11-23T19:47:58+00:00
author: Jan
layout: page
guid: http://www.broad-strokes.com/?page_id=426
---
## Getting Started

After adding this package to your project, you will find a new /ModularWeapons folder in the root directory of the content browser. In the /ModularWeapons/Blueprints folder you’ll find a class called ‘ModularWeapon’, which is the base class that contains most of the logic for using this blueprint system. Add an instance of that class to your level and you can start playing with its properties!

You’ll notice that at first, there is no weapon to be seen. This is because by default, the ‘ModularWeapon’ class generates its mesh components on Begin Play, i.e. when the actor is initialized at runtime, and not during actor construction. In order to preview the weapon (or if you want to use them as decorative objects in the level, set the **Generate On Actor Construction** variable in the ModularWeapon actor’s details panel to true.

If you want to generate a random weapon, set the **Generate Random Weapon** variable to true by ticking the checkbox, and all the parts and material presets of the weapon actor will be randomized. If you want to only randomize the parts or the material presets, use the

## How it works

The Over 9000 Swords modular weapon system uses a coded string to determine what specific weapon and material combination to use when it generates a weapon, which is stored in its **Weapon Code** variable.

The information that this coded string is based on is determined by the ‘WeaponData’ class, which you can find in the /ModularWeapons/Data folder. The default variables of this class contain all of the data used to generate weapons in our system.

When a modular weapon is generated (either on Begin Play, or at construction), it looks at its **Weapon Code** variable, and parses the string to determine what mesh components to create, and how to configure those meshes’ material parameters. For each ‘part’ generated, one static mesh component is added to the actor, and a dynamic material instance is generated.

For an example implementation of a melee weapon with collision detection check out the ‘ModularWeapon_Melee’ class.

## How to configure/extend it

The WeaponData class mentioned above is configured with all of the data for the sword parts and material preset that come with this package. Let’s look at it a bit more closely! We’re only interested in the class defaults here. There are three arrays of structs:

  * **Weapon Types** contains definitions of all available types of weapons. By default, there will only be one item, with a **Weapon Type Name** of “sword”. This struct also contains configuration information for that type’s material parameters.
  
    The **Material Slot Types** array defines the number of “slots” for material presets that each weapon of this type will have, as well as the material preset type that slot will have. By default, our system has three slot types: “Main” (metals), “Deco” (non-metals), and “Energy” (glowy bits). You can add more possible types by editing the “MaterialSlot” enumeration asset located in /ModularWeapons/Data.
  
    The **Material Slot Parameter Names** array is intimately tied to the UE4 Material setup used for mesh parts of this weapon type. The master material that we’re shipping with this package is highly configurable, take a look at it for an example of how you could set up your own!
  * **Weapon Parts** defines the parts used for each weapon type. By default, this array contains the four parts of the swords: blade, guard, hilt, and pommel.
  
    The first part of this struct is the **Weapon Type Name** that this part belongs to. This has to be the exact same as the name defined in the Weapon Types array.
  
    The **Part Prefix** string has to be unique for parts of this weapon type.
  
    The **Meshes** array is a list of all the static mesh assets that can be used by this part. You can easily add or remove meshes here to expand or limit the selection of parts.
  
    Finally, you can define whether a part should be **Optional** or not. Optional parts can be excluded from randomization, or be weighted to only appear with a certain percentage chance during generation.
  * **Material Sets** is a list of all the possible material parameter combinations that can be used. This struct contains a **Material Name**, which can be anything you want (we only use it for the purpose of our example level’s crafting system example; a selector for the **Material Type** (as defined above); a **Unique Short Name** which is used in the weapon code string, as well as some parameters that define the color, roughness, metallic and emissive properties of this material preset. You can easily add new material presets here, or remove ones that you don’t like.
  
    Remember that this list is directly dependent on the material setup used on your meshes, since it only modifies existing material parameters.

## Example Content

Inside the /ModularWeapons folder, there is a subdirectory called GameExample. It contains an example level (/GameExample/Levels/Demo_Examples) that has a few hands-on how-to tutorials and example blueprints showing how you could use this modular weapon system. You can safely delete this folder from your projects once you’re familiar with the system and understand how it works, since the content in it is not referenced by the system itself.

## Closing Remarks

We designed this package to be easy to use, and easy for you to extend, and this document should be a good starting point for you to understand how it works and how to use it. If you think that we could improve the system or this tutorial in any way, please let us know!

If you need help setting up the system to work with your project or need any other further support, we have a dedicated Slack set up for real-time live support that will connect you directly with us, and of course you can always email us with any questions at <support@broad-strokes.com>!