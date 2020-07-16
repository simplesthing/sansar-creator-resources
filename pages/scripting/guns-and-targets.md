---
title:  "Guns and Targets" 
summary: "How to build a shooting gallery."
category: scripting
tags: [simple_scripts, games ]
sidebar: sidebar
permalink: guns-and-targets.html
topnav: topnav
author: Binah
last_updated: September 26, 2019
---

We're going to be looking at the Gun Scripts and then modifying the example Pew Pew script to make a shooting gallery.

The following assets are being used in the post

- [Revolver Firearm Gun
](https://store.sansar.com/listings/54247d28-ae44-48c3-a59a-584f185c5397/revolver-firearm-gun)
- [Military Target](https://store.sansar.com/listings/90e9ed7c-5a0c-4dcc-b1f7-6d2773124ff7/military-target)
- [Breaking Bottle (customizable)](https://store.sansar.com/listings/137a3118-61fd-4cf5-a64f-345d99be4151/breaking-bottle-customizable)
- [Simple Animation with Auto Reset Script](https://store.sansar.com/listings/981d70f7-52d3-40b3-a547-7063b29eaad2/simpleanimationwithautoreset)
- [Hits and impact sounds from fesliyanstudios.com](https://www.fesliyanstudios.com/royalty-free-sound-effects-download/hits-and-impacts-118)
- [Shooting sounds from soundbible.com](http://soundbible.com/tags-shooting.html)

## Guns
First things first, grab a gun, place it in your scene. Set the following toggles:
_note: if you do not see a `Motion Type` setting for your gun you do not have a gun with a collision layer._

![gun settings]({{ site.baseurl }}/images/guns/gun-settings.png)

Add a `Game Gun` script from the `Sansar Script Library` to the gun, add a `shot_fired` and `target_hit` event, also set the clip size to `0` for infinite shots.

![gun script]({{ site.baseurl }}/images/guns/gun-script.png)

Add another `Sansar Script Library` script, `Sound`, for playing the gun shot sound. Link an audio clip and add the `shot_fired` event.

![gun settings]({{ site.baseurl }}/images/guns/gun-simple-sound.png)

Build the scene and visit. If you are in desktop mode you will notice that you can pick up the gun and shoot with the left mouse button but as soon as you do you also drop the gun. If you are in VR you will notice how you can pick up the gun and shoot without dropping it but your hands can hold the gun awkward ways. In order to fix both desktop and VR you can add `Grab Points` to the gun which allow you to position how the object should be held when grabbed as well as toggling `Stay in Hand` for desktop mode so the gun does not drop when the trigger is pulled. 

Right click the object and choose `Add > Grab Points`

![grab point]({{ site.baseurl }}/images/guns/add-grab.png)

You should now see a set of hands positioned somewhere on the object, its very likely the hands will not be on the gun grip and you will need to position and adjust them to hold the gun naturally.

![grab point]({{ site.baseurl }}/images/guns/grab-placement.png)

If you don't see the hands you may need to set the visibility of Grab Points in the top left Visibility menu as shown. 

![grab point]({{ site.baseurl }}/images/guns/grab-visibility.png)

In the `Object Structure` panel select the left and right grab points individually to set the `Stay In Hand` toggle to `On` in the `Properties` panel.

![grab point settings]({{ site.baseurl }}/images/guns/grab-settings.png)

Build and revisit and now you will see in both desktop and VR you do not drop the gun when firing. Although when in desktop mode it is not easy to tell where you are shooting. The fact is you shoot whatever your mouse is pointed at, essentially you are clicking on a thing to shoot it. The fact that the arms stay at your side in desktop is also confusing, I don't have a way to fix the arms at side issue but we can up the game for shooting by toggling the `Free Click Enabled` to `Off`

![free click setting]({{ site.baseurl }}/images/guns/free-click-off.png)

When `Free Click Enabled` is `Off` and you try to click to shoot with the settings above you will get a warning in chat from the System telling the player that they have to be in Mouse Look in order to shoot.

![free click system message]({{ site.baseurl }}/images/guns/free-click-message.png)

[Mouse Look](https://help.sansar.com/hc/en-us/articles/360020523312-Mouselook-mode) mode is a toggle on the escape key which turns your mouse cursor into a circular reticle you can use for aiming. 

![reticle]({{ site.baseurl }}/images/guns/reticle.png)

## Targets

Drag out a target, make sure `Has Collision` and `Is Scriptable` are both toggled to `On`.

![target settings]({{ site.baseurl }}/images/guns/military-settings.png)

Add a `Game Target` script and a `Sound` script from the `Sansar Script Library`

![target settings]({{ site.baseurl }}/images/guns/target-settings.png)

Build and shoot the target, you should hear both a gun fire and a hit sound when you make a shot. If for some reason you don't hear anything don't forget you can always add `Debugger` scripts from the `Sansar Script Library` to log events.

![debugger script]({{ site.baseurl }}/images/guns/debugger.png)

## Pew Pew Example Script

The [Pew Pew Example](https://github.com/lindenlab/sansar-script/blob/master/Examples/PewPewExample.cs) script does many of the things we just did with a single script. [Download](https://raw.githubusercontent.com/lindenlab/sansar-script/master/Examples/PewPewExample.cs) the script and then import it into your scene.

![import script]({{ site.baseurl }}/images/guns/import-script.png)

Remove both scripts on gun the gun and replace with the `PewPewExample` script and choose the `PewPewExample.Gun` option.
The Pew Pew Example script has a lot of the same functionality as the the `Game Gun` script with some nice extras like having sound effects play when players and targets are hit and built in debugging with a toggle. Although I didn't quite understand the difference between the `Shot Hit Sound` and the `Shot Miss Sound` as both sounds to me would just be the same gun shot sound so I just used the same sound twice.

![pew pew example]({{ site.baseurl }}/images/guns/pew-pew-gun.png)

Do the same and remove the scripts from the target as well. The target is simple, it has a hit sound to play a Loudness and a point value when the target is hit.

![pew pew example]({{ site.baseurl }}/images/guns/pew-pew-target.png)

Build and test, it should function almost the same as we had before, but not *exactly*...the things I found missing were 
- the ability to restrict `Free Click` mode, forcing the use of `Mouse Look` for desktop aiming
- the infinite clip when using `0` as clip size
- the ability to send a Target Hit event, which can be used to trigger animations as we'll demonstrate later
- the `Target` class doesn't have any debug logging

I also did not understand the `Shot Hit`/`Shot Miss` sounds, to me it seems like one sound for when a shot is fired and a completely different sound for when a shot is missed and leave the shot hit sound up to the target, maybe that was the intention here but the labelling is not clear so I'd like to change that as well.

![shot sounds]({{ site.baseurl }}/images/guns/shot-sounds.png)

As an added bonus we'll also be adding some actions when a player is hit ....

## Extend Pew Pew Script

### Shot Sound

Starting off with the `PewPewExample.cs` the first thing I'd like to change are the confusing sound inputs. Replace `ShotHitSound` with a more generic `ShotSound` and up date the tooltip to reflect the fact that it wil play when a shot is fired.  

Find the two places the deleted variable is used in the `OnTrigger` methods, both lines which use the variable are identical, they play the sound assigned to `ShotHitSound` copy one of the lines and then delete both lines that use the `ShotHitSound` variable. Instead we want to play the sound each time the `OnTrigger` is called regardless if the shot hits the target or not,paste that line following the `shotsFired++` increment and `ammo--` decrement and update the `ShotHitSound` variable to `ShoutSound`.

[*see commit changes*](https://github.com/lindenlab/sansar-script/pull/22/commits/0f635c85674ea0bb4ed73a4ae748b3b598726810){:target="_blank"}
![script sound changes]({{ site.baseurl }}/images/guns/sound-changes.png)

### Infinite Clip

Next lets re-enable infinite clip size with a value of `0`. Update the tooltip and set the default value to zero.

Inside the `Reload` method add an if statement that checks for the value of `0` in `ClipSize` and if so return out out of the function. 

In the `OnTrigger` method find where the ammo check happens and add another clause to the if statement ensuring that `ClipSize` is greater than `0`.

[*see commit changes*](https://github.com/lindenlab/sansar-script/pull/22/commits/d7e65ef6743662457171174798a1e7288b0c49b5){:target="_blank"}
![infinite clip script]({{ site.baseurl }}/images/guns/infinite-clip.png)

### Disable Free Click

Now lets disable the ability to `Free Click` on the desktop to fire the gun, and enforce the use of `Mouse Look` to aim and fire.

Start by adding a public configurable property for `FreeClickEnabled` and default it to `false`.

[Command Data](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/CommandData.html#P:Sansar.Simulation.CommandData.ControlPoint) is sent when an input event occurs, and it sends with it a [Control Point Type](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/ControlPointType.html), an enum type that include `DesktopGrab` and `LeftTool` and `RightTool` for VR, as possible values.

In the `OnTrigger` method you will want to add a check for which `ControlPointType` is passed through `CommandData` and do a check on the type. If the type is `DesktopGrab` then do a check if `FreeClickEnabled` is `false`, if it is also check that `MouseLookMode` is enabled before allowing execution to continue, if it is not send a message to the user through chat telling them they must be on Mouse Look mode to use the gun.

[*see commit changes*](https://github.com/lindenlab/sansar-script/pull/22/commits/9b03750aa640dc2bf5c7bfbfb31280396677b624){:target="_blank"}
![free click toggle script]({{ site.baseurl }}/images/guns/free-click-toggle.png)

### Hit Event

Let's add a `ShotHitEvent` when a target has been hit to do things like trigger animations through simple scripts. 

Add a public property and tooltip for `ShotHitEvent` in the `Target` class. Also add one for `DebugLogging`.

Add the [simple script interface](https://github.com/lindenlab/sansar-script/blob/master/Examples/ScriptLibrary/LibraryBase.cs#L17) to the `Target` class. In the `Hit` method compose and send that event, along with a debug log.

[*see commit changes*](https://github.com/lindenlab/sansar-script/pull/22/commits/c39c7a8ff292bf068984ff198be2bbdc33237241){:target="_blank"}
![target hit event]({{ site.baseurl }}/images/guns/target-hit-event.png)

Build and visit, open up the debug panel and shoot your target.

![target hit]({{ site.baseurl }}/images/guns/target-hit.png)

## Build a shooting gallery

Now that we have our script doing all the things we want it to do, lets put it all together and make a simple shooting gallery. 

Lets start by adding `Shot Miss Sound` to the gun. Once you have the gun set up the way you like it, place it in world to be picked, duplicate as many guns as you want to have in your gallery.

![gun table]({{ site.baseurl }}/images/guns/gun-table.png)

Do the same for the military target, make sure the script is setup how you like it and duplicate a few times and spread them out, leave room for some exploding bottles as well.

![military targets]({{ site.baseurl }}/images/guns/military-targets.png)

Lets get some exploding targets going as well, a breaking [bottle](https://store.sansar.com/listings/137a3118-61fd-4cf5-a64f-345d99be4151/breaking-bottle-customizable) from the store.

Starting with the bottle drag it out and set the initial settings `Is Scriptable` to `On`, `Play back Mode` to `SinglePlay` and `Begin On Load` to `Off`.

![initial bottle settings]({{ site.baseurl }}/images/guns/bottle-initial.png)

For this object I found that I needed to also adjust the volume component in order to make the target script work, by setting `Volume Type` to `Box` and resizing and positioning over the mesh. 

![bottle volume settings]({{ site.baseurl }}/images/guns/bottle-volume.png)

At this point save the bottle to your inventory and give it a distinct name.

![save to inventory]({{ site.baseurl }}/images/guns/save-to-inventory.png)

Add the PewPewExtended script and enter a target event and a sound.

![bottle pew pew settings]({{ site.baseurl }}/images/guns/bottle-pew-pew.png)

Next add the [Animation Reset Script](https://store.sansar.com/listings/981d70f7-52d3-40b3-a547-7063b29eaad2/simpleanimationwithautoreset) from the same creators of the breaking bottle to reset  the animation a few seconds after the animation has played (read bottle breaking listing for animation frame details).

![bottle pew pew settings]({{ site.baseurl }}/images/guns/simple-animation-reset.png)

Run a build and test out, you should be breaking the bottles on hit and a few seconds later they reset to their full bottle form. 