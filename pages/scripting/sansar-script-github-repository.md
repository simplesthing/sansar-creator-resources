﻿---
title:  "Sansar Scripting Documentation"
summary: "Readme information and link to sansar-script github repository"
category: scripting
tags: [ getting_started ]
sidebar: sidebar
permalink: sansar-script.html
topnav: topnav
author: "Leslie Linden"
last_updated: July 8, 2019
---

## sansar-script github repository link

This is a link to the official [Sansar scripting open source repository](https://github.com/lindenlab/sansar-script) where you find offcial Sansar scripting documentation, the information below has been copied from the README in that repository, you will find the same content plus script examples by following [this link](https://github.com/lindenlab/sansar-script). 

## Introduction

Hello and welcome!

This public repository contains C# scripts and assets intended for use within Sansar. Inside you will find examples, samples, tutorials and more.

Some materials are intended for people new to Sansar and new to C# while others will likely only make sense to seasoned developers.

The easiest way to create interactive content in Sansar is to make use of the built-in "Scene Scripts Library". This library was formerly referred to as "Simple Scripts" so you may notice that term used instead. The instructions on this page do not focus on the use of these built-in scripts, but are instead intended to help creators write their own scripts.

Below the table of contents, there are instructions on how to start making experiences with scripts. They can also be handy for anyone trying to get any of the included examples or tutorials in this repository.

And further down on the page is an introduction to scripting in Sansar. This assumes a basic knowledge of modern programming principles and C# and can be quite technical at times. But there is no need to fully understand all of this.

Get in and start tinkering and you will probably be able to sort out how to do many things without needing to be an expert in C#!

Also all are encouraged to ask questions and follow the #scripting channel in Sansar Official Discord!

Happy scripting!

# File structure for this repository

**Examples** - contains a copy of the example scripts that are distributed with Sansar.  By default
these can be found in your `C:\Program Files\Sansar\Client\ScriptApi\Examples` directory.

**Samples** - contains all of the snippets of code from the documentation here, saved out as individual
files for ease-of-use.

**Tutorials** - hopefully this is self-explanatory enough.  Look in this directory if you're trying to
get started.
Tutorials are different than examples in that they may include binary assets or other resources you may
need to follow along.

**Users** - this is the dumping ground for scripts from individuals within and external to the Sansar
development team.


# Scripting documentation

The full API documentation comes with the Sansar installation and should be available for most users here:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\index.html`

If you installed Sansar to a different directory you'll need to browse for the documentation in your Sansar folder.

These docs can be intimidating at first but once you get the hang of where to find things you will get
used to using them as a reference.


## Brief summary of Sansar namespaces

The main namespaces in the Sansar scripting system are `Sansar`, `Sansar.Script` and `Sansar.Simulation`.

The main `Sansar` namespace includes the base math constants and functions in `Mathf` and a few types
for working with colors (`Color`), rotations (`Quaternion`) and vectors (`Vector`).

The `Sansar.Script` namespace includes the base types used throughout the script API and access to
features such as log messages for debugging.  

Notable `Sansar.Script` interfaces include:
 
* **`Log.Write`** - print messages to the debug console (use Ctrl+d to view)
* **`ComponentId`** - struct that uniquely identifies a component on a particular object
* **`ObjectId`** - struct to uniquely identify an object
* **`SessionId`** - struct to uniquely identify a user for a single visit to your experience
* **`ICoroutine`** - interface for signalling or aborting coroutines
* **`Timer`** - for one-time or repeating events

`Sansar.Simulation` is the namespace that contains all of the heavy weight classes for working with
avatars (known as 'agents'), the scene itself and all of the various components on objects in the scene.
If you're having trouble finding something in the script API, it is most likely in this namespace
somewhere.

Notable `Sansar.Simulation` interfaces include:

* **`AgentPrivate`** - this is the main class for interaction with the avatars visiting your scene, such
 as playing sounds, getting hand positions and sending direct messages.  Also the `AgentInfo` struct
 includes the unique `AvatarUuid` to identify a player.
* **`AgentPrivate.Client`** - for handling input and teleporting avatars around
* **`AgentPrivate.Client.UI.ModalDialog`** - for modal text windows with one or two buttons.
* **`ObjectPrivate`** - access object position and retrieve components
* **`ObjectPrivate.Mover`** - for moving non-physical objects around
* **`ScenePrivate`** - find agents, other scripts, objects, spawn things, adjust gravity, etc.
* **`ScenePrivate.Chat`** - general nearby chat
* **`SceneObjectScript`** - base class for most scripts

In addition to those, `Sansar.Simulation` also includes the following components:

* **`AnimationComponent`** - for controlling animations on animated objects
* **`AudioComponent`** - for controlling sounds on objects
* **`LightComponent`** - for controlling lights
* **`RigidBodyComponent`** - to adjust physics properties or apply forces or otherwise move physical objects


## AgentPrivate vs. AgentPublic, etc.

If you look at the documentation you will notice that `AgentPrivate` has a corresponding `AgentPublic`
and `ScenePrivate` has a corresponding `ScenePublic`.  These are in place for a future time when
people will be able to attach scripts to their own avatars and take them into other people's scenes
and they can be largely ignored for now.  But the idea is that scene creators will have more access
to info and more ability to manipulate and probe the scene than scripts brought in by users.

You can note, for example, if you make your script class derive from `ObjectScript` instead of
`SceneObjectScript` that it will only have access to `ScenePublic` instead of `ScenePrivate`, which
has a far more limited interface.


# How to create a scripted experience in Sansar

The first step to creating a new experience in Sansar is to find the Build menu and create a new
experience.  I would suggest starting from the Base Template so you have a scene that you can build
and rebuild quickly, which often saves a lot of time while working on scripts.

Once you are editing an experience, the first step for scripts is to import them into your inventory.
After that you can hook them up to specific objects in your scene by adding a script component to the
object or by dragging the script out of your inventory and dropping it on the object in your scene.

After that, you need to build your scene to see your script in action!

## Importing

1. Download an example script or tutorial from this repository.
2. In the Sansar Editor, choose:
  **Import** -> **Script**  and choose the script.
3. Hit the red import button.

This will compile the script and import it to your inventory.  To find it, make sure your inventory
is open by going to:
  **Tools** -> **Inventory**  at the top of the screen.

You may also need to make sure your filtering options are set to see your own creations.  To do this
change the Inventory window first filter to "Created" or "All sources" and the second filter to 
"Scripts" or "All types".  If the last filter is "Newest" then the script you just imported should
now be in the top left corner of the Inventory window.

## Attaching a script to an object

There are several ways to attach a script to an object in the world.  The basic method is to drag
the script from your inventory and drop it onto the object.  This will automatically add a new script
component to the object and assign it to use your script.

Alternatively, you can right click on an object in the world or in the "Scene Objects" view and choose:
  **Add** -> **Script**  to add a script component to your object.
Then you will need to look at the Properties panel for that object, find the script component and set
the "Script" property to your newly imported script.

## Building the scene

Once the script is attached to an object in the scene and you are ready to test it, hit the "Build"
button to save, build and upload your scene.  Then choose the "Visit Now" option to see it in action.

When you have tested it and are ready to go back to the editor to make further changes, go back to
the "Create" menu on the left-hand side and choose the "Edit this scene" option.

Depending on scene complexity, you may find this build/play loop takes a while.  Make sure to change
the following scene settings to make your iteration time as fast as possible:

  **Tools** -> **Scene settings**
  *  ->  **Light**:  Change "Global Illum Quality" to "No processing"
  *  ->  **Background Sound**:  Change "Compute Reverb" to "Off"


# Getting started with scripting

What follows here is a quick summary that might help you get started a bit faster if you are
already familiar with C# syntax and object oriented programming principles.

Most scripts will want to define a new class and derive from the `SceneObjectScript` base class in
the `Sansar.Simulation` namespace like so:

```c#
public class ExampleScript : Sansar.Simulation.SceneObjectScript
{
    public override void Init() {}
}
```

To save you from having to type `Sansar.Simulation` everywhere you'll probably want to 
start most files with a using statement or two:

```c#
using Sansar.Simulation;

public class ExampleScript : SceneObjectScript
{
    public override void Init() {}
}
```

That's technically a full Sansar script!  However, since it does absolutely nothing at all let's
go ahead and update it before we try using it in a scene:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class HelloSansarScript : SceneObjectScript
{
    public override void Init()
    {
        Log.Write("Hello Sansar!");
    }
}
```

So as you can see, the `Sansar.Script` namespace includes a log function to print out messages.  
These messages go to the debug console and are only visible to the owner of the scene.

So if we drop that into the example above we will get a script that will write a message to the
debug console at script initialization time.  Go ahead and try this out and read on to find out
how to see these messages!


## How to use the debug console

See the above instructions on how to import the script and attach it to an object in the scene.
Then when you build and run the scene and press `Ctrl+d` you will see `Hello Sansar!` in the
console.

This logging functionality can also be used to write yellow warning text and red error text to
the debug console like so:

```c#
Log.Write(LogLevel.Info, "Information text");
Log.Write(LogLevel.Warning, "Yellow warning text");
Log.Write(LogLevel.Error, "Red error text");
```

For the complete set of logging functionaly check the API documentation in your Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Script\Log.html`


## How to create properties that can be modified in the editor

In general, public script variables will show up in the editor on the properties panel for the
object that has the script on it.  More specifically there are a limited set of property types
that are supported by the Sansar editor.  Here is a sample script with one of each type of
supported property:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class PropertiesExampleScript : SceneObjectScript
{
    public bool BoolProperty;
    public int IntProperty;
    public float FloatProperty;
    public double DoubleProperty;
    public string StringProperty;
    public Vector VectorProperty;
    public Quaternion QuaternionProperty;
    public Color ColorProperty;
    public Interaction InteractionProperty;
    public ClusterResource ClusterResourceProperty;
    public SoundResource SoundResourceProperty;

    public override void Init() {}
}
```

Note the addition of the `using Sansar;` line so that all of these types can be referenced without
further qualification.

### Property metadata

In addition there are custom C# properties that can be used to set default values, assign ranges,
override the display name and define a tooltip for each property.  Here are a few examples of these:

```c#
    [Tooltip("The tooltip for this string property")]
    [DefaultValue("default string")]
    [DisplayName("The String Property")]
    public string MyStringProperty;

    [DefaultValue(3)]
    [Range(0,5)
    public int MyRangedIntProperty;

    [Tooltip("Custom object gravity multiplier")]
    [DefaultValue(1.0f)]
    [Range(-2.0f, 2.0f)]
    [DisplayName("Gravity Multiplier")]
    public float GravityFactor;

    [Tooltip("The pivot point of the rotation, in object local space.")]
    [DisplayName("Object Rotation Pivot")]
    [DefaultValue(0,0,1)]
    public Vector RotationPivot;

    [Tooltip("The color of the light for Mode A")]
    [DisplayName("Mode A Color")]
    [DefaultValue(1,0.8,0.5,1)]
    public Sansar.Color ColorModeA;
```

Quaternions are also supported but you most likely would want to use a 
`Vector` for a rotation property and then initialize your script rotation from Euler angles since 
typing in Quaternion does not come easily to most users.  This could be done like so:
```c#
Quaternion q = Quaternion.FromEulerAngles(Mathf.RadiansPerDegree * MyVectorRotationProperty);
```

The `Interaction` type above is a string in the editor but also enables the object to be clickable
in world.  More on that below.

The `ClusterResource` and `SoundResource` properties allow scripts to interact with and spawn other
objects in your inventory.  There is no way to specify default values for these types and the editor
will present the list of objects of that type in the user's inventory.

### Limits and arrays

Scripts have a maximum number of properties that can be exposed to the editor.  The exact number is
dependent on the base class type for your script.

For the examples above that derive from `SceneObjectScript` the limit is 20 properties.
Scripts that derive from `ObjectScript` have a limit of 10 properties.

One way to get beyond the 20 property limit is to use array types.  The syntax for these
uses the C# `List` like so:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;
using System.Collections.Generic;

public class ArrayPropertiesExampleScript : SceneObjectScript
{
    public List<bool> BoolValues;
    public List<int> IntValues;
    public List<float> FloatValues;
    // etc.

    public override void Init() {}
}
```

Note that all of the property types previously introduced will work as arrays although I would not
recommend using the `Interaction` type in an array since only one will function on an object!


## How to see text in world

There are a few different ways to get messages from a script into the world, depending on what you
are trying to do.

### Debug console messages

Messages can be written to the debug console (Ctrl+d) using the `Log.Write` function.  These are
only visible to you, the creator and owner of the scene.

### Nearby chat messages

Messages can be broadcast to nearby chat via `ScenePrivate.Chat.MessageAllUsers`.

### Direct messages and private messages

Messages can be sent to an individual as a direct message from `agent.SendChat` where `agent` is an
instance of `AgentPrivate`.  Usually the `agent` is obtained from a `SessionId` that comes as part
of the data from an event callback.  For example using the above `InteractionProperty` that would be:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);
    if (agent != null)
        agent.SendChat("Hello from script!");
});
```

### Modal dialogs

Messages can also be presented to the user in a modal dialog like so:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);
    if (agent != null) {
        agent.Client.UI.ModalDialog.Show("Choose your destiny!", "No", "Yes!", (opc) =>
        {
            if (agent.Client.UI.ModalDialog.Response == "Yes!")
                agent.SendChat("You pressed yes!");
            else
                agent.SendChat("You pressed no.");
        });
    }
});
```

### In-world interaction text

And also interactions can have custom and changeable prompt messages that show up as mouse-over, hover text
in-world.  See below for details on interaction.


## How to make something clickable using Interaction

Objects that have an `Interaction` property will be clickable in-world by default for all users:

```c#
public Interaction MyInteraction;
```

This will display hover text and show a green highlight on the object when a user is pointing at it.  The 
hover text can be configured in the editor by editing the `MyInteraction` field on the properties panel.

Note that objects can only have a single interaction property but the interaction hover text can be changed
on the fly using the `Interaction.SetPrompt` function.

Lastly, scripts can add a custom interaction at runtime if they wish.

```c#
using Sansar.Script;
using Sansar.Simulation;

public class AddInteractionScript : SceneObjectScript
{
    public string Title;

    public override void Init()
    {
        ObjectPrivate.AddInteractionData addData = (ObjectPrivate.AddInteractionData) WaitFor(ObjectPrivate.AddInteraction, Title, true);

        addData.Interaction.Subscribe((InteractionData data) =>
        {
            AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);

            if (agent != null)
            {
                agent.SendChat($"Hello from {Title}!");
            }
        });
    }
}
```

Also note that both the interaction text and the interaction itself can be customized globally or per user.

For the complete set of functionality related to Interaction, check the API documentation in the Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\Interaction.html`


## How to respond to a button press

The way to have code that executes when a keyboard, mouse or controller button is pressed is to subscribe to
a command on an agent's client.  Common commands to listen for include "Trigger", "PrimaryAction" and 
"SecondaryAction".

The complete list of supported commands can be found in the API documentation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\CommandData.html`

Since the callback is on an agent in the scene, we must first get to the agent somehow.  This can be done
for all users in a scene by listening for the `User.AddUser` event on the scene itself, or it could be done
more specifically by waiting for an agent to enter a trigger volume, press a button or pick up an object.


### Respond to a button press for all users in a scene

Scripts might not always be running right from the very start of a scene so tracking commands from all
users in a scene requires subscribing to commands from the existing users already in the scene and
listening for new users as they enter the scene to subscribe to their commands as well.

This can be done as follows:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class AllAgentsCommandScript : SceneObjectScript
{
    public override void Init()
    {
        // Find each user as they enter the scene and listen for commands from them
        ScenePrivate.User.Subscribe(User.AddUser, (UserData ud) =>
        {
            AgentPrivate agent = ScenePrivate.FindAgent(ud.User);
            if (agent != null)
                ListenForCommand(agent);
        });

        // Listen for commands from any users already in the scene
        foreach (var agent in ScenePrivate.GetAgents())
            ListenForCommand(agent);
    }

    void ListenForCommand(AgentPrivate agent)
    {
        agent.Client.SubscribeToCommand("Trigger", CommandAction.Pressed, (CommandData command) =>
        {
            Log.Write(agent.AgentInfo.Name + " pressed trigger!");
        },
        (canceledData) => { });
    }
}
```


### Respond to a button press only when a user is holding an object

Using the held object callbacks on a rigidbody allows the script to track and respond when it is being held.
This would be a typical setup for a flashlight or gun to respond to a command while being carried around:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class HeldObjectCommandScript : SceneObjectScript
{
    RigidBodyComponent _rb = null;
    IEventSubscription _commandSubscription = null;

    public override void Init()
    {
        if (!ObjectPrivate.TryGetFirstComponent(out _rb))
        {
            Log.Write(LogLevel.Error, "HeldObjectCommandScript can't find a RigidBodyComponent!");
            return;
        }

        if (!_rb.GetCanGrab())
        {
            Log.Write(LogLevel.Error, "HeldObjectCommandScript isn't on a grabbable object!");
            return;
        }

        // Subscribe to the "PrimaryAction" action when the object is picked up
        _rb.SubscribeToHeldObject(HeldObjectEventType.Grab, (HeldObjectData holdData) =>
        {
            AgentPrivate agent = ScenePrivate.FindAgent(holdData.HeldObjectInfo.SessionId);

            if (agent != null && agent.IsValid)
            {
                _commandSubscription = agent.Client.SubscribeToCommand("PrimaryAction", CommandAction.Pressed, (CommandData command) =>
                {
                    Log.Write(agent.AgentInfo.Name + " pressed the button while holding the object!");
                },
                (canceledData) => { });
            }
        });

        // Unsubscribe from the "PrimaryAction" action when the object is dropped
        _rb.SubscribeToHeldObject(HeldObjectEventType.Release, (HeldObjectData holdData) =>
        {
            if (_commandSubscription != null)
            {
                _commandSubscription.Unsubscribe();
                _commandSubscription = null;
            }
        });
    }
}
```


### Using the targeting information in a command

It can often be useful to know what object an agent was pointing at when they performed a command.  If the
script were to respond to the command by doing a raycast, the relevant object may be out of the way and the
delay in script processing caused by latency between the server and the client may create a frustrating
experience for the user.  So for guns or other systems hoping to provide a faster response, it is highly
recommended to use the `CommandData.TargetingComponent` field.

This can be done like so:

```c#
agent.Client.SubscribeToCommand("Trigger", CommandAction.Pressed, OnTrigger, (canceledData) => { });

void OnTrigger(CommandData command)
{
    if (command.Action != CommandAction.Pressed)
        return;

    ObjectPrivate targetObject = ScenePrivate.FindObject(command.TargetingComponent.ObjectId);
    if (targetObject != null)
    {
        // Player was pointing at targetObject when they pressed "Trigger"
    }
}
```

Please also look further down in these docs to find the section on raycasts and shapecasts for more options
on how to query the physics collision shapes in the scene.

The complete set of data for commands can be found here:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\CommandData.html`


## How to control animations

Sansar supports animations in model FBX files.  These are authored and exported from Maya, Blender, etc. and then
imported to Sansar.  These files can contain one or more animations which can all be accessed and controlled by the
scripting API using the `Sansar.Simulation.AnimationComponent`.

Note:  All animations imported to Sansar are resampled to 30fps.  You will want to export animations at 30fps if you
plan to have precise frame control from your code.

The first task when working with the components in Sansar is to acquire the component from the object.  This can be
done from a few different API endpoints but a common way is as follows:

```c#
AnimationComponent animComp;
ObjectPrivate.TryGetFirstComponent(out animComp);
```

If the script is running on an object that does not have animations, the above code will fail to acquire an animation
component.  A script can safely playback an animation if it exists with a little error checking:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class AnimationScript : SceneObjectScript
{
    public override void Init()
    {
        AnimationComponent animComp;
        if (ObjectPrivate.TryGetFirstComponent(out animComp))
            animComp.DefaultAnimation.Play();
        else
            Log.Write(LogLevel.Warning, "AnimationScript not running on an animated object!");
    }
}
```

As you can see above, a quick way to access the animation from the `AnimationComponent` instance is through the 
`DefaultAnimation` member.  

If you wish to control playback more precisely, your script will need to use the `AnimationParameters` struct.  
For example if you wanted to loop a certain animation from frame 10 to frame 50, playback would be done like so:

```c#
AnimationParameters animParams = animComp.DefaultAnimation.GetParameters();
animParams.PlaybackMode = AnimationPlaybackMode.Loop;
animParams.ClampToRange = true;
animParams.RangeStartFrame = 10;
animParams.RangeEndFrame = 50;
animComp.DefaultAnimation.Play(animParams);
```

You can also create a new animation parameters struct if you prefer but getting the parameters from the animation
as is done above will preserve any other editor settings.  In this case, notably the playback speed from the editor
would be preserved since the code is not overwriting that member.  This will allow you to adjust the playback speed
without having to recompile the script.

If the object has multiple animations in the FBX, your script will need to use the 
`GetAnimation(string)` or `GetAnimations()` interface instead of simply accessing the `DefaultAnimation`.

For the complete set of functionaly related to animations, check the API documentation
in your Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\AnimationComponent.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\Animation.html`


## How to turn lights on and off

Lights, like animation, has its own component for script control namely the `LightComponent`.  Acquiring access
to the light component can be done in a very similar way:

```c#
LightComponent lightComp;
ObjectPrivate.TryGetFirstComponent(out lightComp);
```

There are a few things to keep in mind with lights.  Unlike animations that exist for the sake of movement, 
lights can potentially be optimized in the scene build process.  Because of this, it is necessary to set the
"Scriptable" flag "On" in the editor for each light that your script may want to adjust.  Your script can
detect whether a light is scriptable using the `IsScriptable` flag.

Here is a script that can safely turn a light off on an object:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class LightOffScript : SceneObjectScript
{
    public override void Init()
    {
        LightComponent lightComp;
        if (!ObjectPrivate.TryGetFirstComponent(out lightComp))
            Log.Write(LogLevel.Warning, "LightScript not running on an object with a light!");
        else if (!lightComp.IsScriptable)
            Log.Write(LogLevel.Warning, "LightScript not running on an object with a scriptable light!");
        else
            lightComp.SetColorAndIntensity(Color.Black, 0.0f);  // turn the light "off"
    }
}
```

Lights in Sansar combine the color and intensity into a single value that is applied to the scene.  
As a result, the script API does not have the ability to manipulate them independently.  Also the color
you apply to a light might be different than the color you retrieve when you query the light but that
comes with the territory.  The script API uses the nomenclature `GetNormalizedColor` and 
`GetRelativeIntensity` vs. `SetColorAndIntensity` to reinforce that idea.

Note that shadow casting lights are expensive to render and can be a considerable performance drain on
lower end machines.  Consider reducing a scene to a single shadow casting light if you or your users
are experiencing a low framerate.

For the complete set of functionaly related to light components, check the API documentation
in your Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\LightComponent.html`


## How to control physical objects

Physical objects in Sansar are those that have physics collision and can be controlled by the physics
engine.  In the editor properties or object structure, this will appear as a "volume" and the object
itself will have a "motion type" property, along with "density" and "friction".

In the script API, all physics objects are manipulated from the `RigidBodyComponent`.  Similarly to
the other components, a common way to get access to the rigid body component is as follows:

```c#
RigidBodyComponent rbComp;
ObjectPrivate.TryGetFirstComponent(out rbComp);
```

A slightly more robust script that will apply an upwards impulse to a dynamic physics object when it
is clicked would be as follows:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class RigidBodyImpulseScript : SceneObjectScript
{
    public Interaction MyInteraction;

    private RigidBodyComponent _rb;

    public override void Init()
    {
        if (ObjectPrivate.TryGetFirstComponent(out _rb))
        {
            _rb.SetCanGrab(false);  // Disable grabbing for this object

            if (_rb.GetMotionType() == RigidBodyMotionType.MotionTypeDynamic)
            {
                MyInteraction.Subscribe((InteractionData data) =>
                {
                    _rb.AddLinearImpulse(Vector.Up * 100.0f);
                });
            }
            else
                Log.Write(LogLevel.Warning, "RigidBodyImpulseScript not running on a dynamic object!");
        }
        else
            Log.Write(LogLevel.Warning, "RigidBodyImpulseScript not running on an object with a physics volume!");
    }
}
```

For the complete set of functionaly related to rigid body components, check the API documentation
in your Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\RigidBodyComponent.html`

### The thing about motion type

The "motion type" property is extremely important when it comes to physics objects and it greatly
affects how the object behaves and what can be done to the object from the script API.  

The "static" motion type indicates that the object will not ever be moving.  This is considered the
most restrictive motion type.  In fact static objects built into the scene are assumed never to move
and are potentially optimized by the build process, so script manipulation of static objects is not
possible.

The "keyframed" motion type indicates that the object will only move when explicitly moved from
script.  This is the next most restrictive motion type.  When keyframed objects are moved, 
they will not stop or otherwise be affected by any other collisions.  They will simply move through 
everything and push avatars and dynamic objects out of the way.

The "dynamic" motion type indicates that the object will be subject to gravity and other phsyical
interactions.  This is the least restrictive motion type.  Dynamic objects will fall, roll and
slide depending on what forces get applied to them in the scene.  They will collide with static
and keyframed objects and avatars.

Motion types can be changed from script but only to a more restrictive motion type than the initial
import or scene settings allow.  So for example, an object imported as "dynamic" can be set to
"keyframed" from script but an object imported as "static" can not be set to "keyframed" or "dynamic".


## How to move non-physical objects

Objects that have no collision are considered non-physical objects in Sansar.  These can be moved
with the Mover API if they are configured to be allowed to move.  Much like "static" objects above,
objects that are not configured for movement are assumed never to move and are potentially optimized 
by the build process.

To configure an object for movement, set the "Movable From Script" attribute to "On".

Also note that the Mover API can drive "keyframed" physics objects but can not drive "dynamic" or "static"
physics objects.  So if your object does have a physics volume and you wish to configure it for movement,
make sure to set the motion type to "keyframed" as well as the "Movable From Script" attribute.

Properly configured objects can then be immediately moved using any of these functions:

```c#
ObjectPrivate.Mover.AddMove(position, rotation);
ObjectPrivate.Mover.AddTranslate(position);
ObjectPrivate.Mover.AddRotate(rotation);
```

The above functions will result in immediate movement from the object's current position and orientation
to the specified location.  Use the following interfaces if you prefer to have more gradual movement over
time:

```c#
ObjectPrivate.Mover.AddMove(position, rotation, seconds, moveMode);
ObjectPrivate.Mover.AddTranslate(position, seconds, moveMode);
ObjectPrivate.Mover.AddRotate(rotation, seconds, moveMode);
```

The additional arguments are the length of time and specified mode for the movement.  Exactly which
move mode you choose will depend on the use case but the options are 
"linear", "ease-in", "ease-out" and "smoothstep".

Note that the mover functions like a queue behind the scenes and executes all commands sequentially.
In this way it is possible to make a simple behavior to move an object through a set of points:

```c#
ObjectPrivate.Mover.AddTranslate(point1, 5.0, MoveMode.Linear);
ObjectPrivate.Mover.AddTranslate(point2, 5.0, MoveMode.Linear);
ObjectPrivate.Mover.AddTranslate(point3, 5.0, MoveMode.Linear);
ObjectPrivate.Mover.AddTranslate(point4, 5.0, MoveMode.Linear);
```

In addition, the `WaitFor` function will run the instructions in a coroutine until the movement has
completed.  So an object can be safely set to patrol around a few points indefinitely like so:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class PatrolMoverScript : SceneObjectScript
{
    public Vector Point1;
    public Vector Point2;
    public Vector Point3;
    public Vector Point4;

    [DefaultValue(1.0)]
    public double MoveTime;

    public override void Init()
    {
        StartCoroutine(PatrolUpdate);
    }

    void PatrolUpdate()
    {
        while (true)
        {
            ObjectPrivate.Mover.AddTranslate(Point1, MoveTime, MoveMode.Linear);
            ObjectPrivate.Mover.AddTranslate(Point2, MoveTime, MoveMode.Linear);
            ObjectPrivate.Mover.AddTranslate(Point3, MoveTime, MoveMode.Linear);
            WaitFor(ObjectPrivate.Mover.AddTranslate, Point4, MoveTime, MoveMode.Linear);
        }
    }
}
```

A slightly fancier version to rotate the object to face its next goal position before translation
and move the object at a constanst speed might be something like this:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;
using System.Collections.Generic;

public class PatrolTurnMoverScript : SceneObjectScript
{
    public List<Vector> PatrolPoints;

    [DefaultValue(1.0f)]
    public float MoveSpeed;

    [DefaultValue("<0,1,0>")]
    public Vector WorldObjectForward;

    public override void Init()
    {
        if ((PatrolPoints.Count > 1) && (MoveSpeed > 0.0f))
            StartCoroutine(PatrolUpdate);
    }

    void PatrolUpdate()
    {
        int current = 0;
        int next = current + 1;

        // Start the object on the first patrol point
        ObjectPrivate.Mover.AddTranslate(PatrolPoints[current]);

        while (true)
        {
            // Calculate direction to next patrol point
            Vector toNext = PatrolPoints[next] - PatrolPoints[current];

            // Compute a world space rotation for this object to point at the next patrol point
            Quaternion rotation = Quaternion.ShortestRotation(WorldObjectForward, toNext.Normalized());
            ObjectPrivate.Mover.AddRotate(rotation);  // Immediately turn to face

            // Compute the time based on the distance and move speed
            double moveTime = toNext.Length() / MoveSpeed;

            // Move the object to the next patrol point
            WaitFor(ObjectPrivate.Mover.AddTranslate, PatrolPoints[next], moveTime, MoveMode.Linear);

            // Increment to the next patrol point
            current = next;
            next = (next + 1) % PatrolPoints.Count;
        }
    }
}
```

The queue can also be cleared using the `StopAndClear` function to interrupt the movement.

For the full interface, check the API documentation in your Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\Mover.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\MoveMode.html`


## How to play sounds

There are a variety of ways to play sounds in Sansar.  Sounds can be played through a specific audio emitter
in a scene, or they can be played spatialized or non-spatialized for a target avatar or for everyone in the scene.

Spatialized sound is sound that comes from a specific position in the world.  It is directional and may not
be heard by avatars that are far away.  Non-spatialized sounds are played at equal volume regardless of where
the listener might be in the scene.

Scripts can not directly reference audio resources without configuration from the editor.  Here is a sample
script to play a sound when an object is clicked:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class SoundScript : SceneObjectScript
{
    public SoundResource Sound;

    [DefaultValue(50.0f)]
    [Range(0.0f, 100.0f)]
    public float Loudness;

    public override void Init()
    {
        // Check to make sure a sound has been configured in the editor
        if (Sound == null)
        {
            Log.Write("SoundScript has no configured sound to play!");
            return;
        }

        ObjectPrivate.AddInteractionData addData = (ObjectPrivate.AddInteractionData) WaitFor(ObjectPrivate.AddInteraction, "Play sound", true);

        addData.Interaction.Subscribe((InteractionData data) =>
        {
            PlaySettings playSettings = PlaySettings.PlayOnce;
            playSettings.Loudness = (60.0f * (Loudness / 100.0f)) - 48.0f;  // Convert percentage to decibels (dB)

            ScenePrivate.PlaySound(Sound, playSettings);
        });
    }
}
```

In this example we are playing the sound in a non-spatialized way for all users in the scene.  Here are alternate
ways to play back the sound for a specific user or spatialized for all users:

```c#
ScenePrivate.PlaySoundAtPosition(Sound, someVector, playSettings);  // spatialized, audible for all

agent.PlaySound(Sound, playSettings);  // non-spatialized, only for the agent
agent.PlaySoundAtPosition(Sound, someVector, playSettings);  // spatialized, only for the agent
```

Playback on a specific audio emitter in the scene requires acquiring the corresponding `AudioComponent`.  Since
this is already familiar to you from the animation, light and rigid body component examples above, let's also expand
this example to start and stop a looping sound, which introduces the concept of a play handle:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class LoopingSoundComponentScript : SceneObjectScript
{
    public SoundResource LoopingSound;

    [DefaultValue(50.0f)]
    [Range(0.0f, 100.0f)]
    public float Loudness;

    private AudioComponent _audio = null;
    private PlayHandle _playHandle = null;

    public override void Init()
    {
        // Check to make sure a sound has been configured in the editor
        if (LoopingSound == null)
        {
            Log.Write("LoopingSoundComponentScript has no configured sound to play!");
            return;
        }

        if (!ObjectPrivate.TryGetFirstComponent(out _audio))
        {
            Log.Write("LoopingSoundComponentScript is on an object that does not have an audio emitter.");
            return;
        }

        ObjectPrivate.AddInteractionData addData = (ObjectPrivate.AddInteractionData) WaitFor(ObjectPrivate.AddInteraction, "Play sound", true);

        addData.Interaction.Subscribe((InteractionData data) =>
        {
            // If not sound is playing, start one up
            if (_playHandle == null)
            {
                PlaySettings playSettings = PlaySettings.Looped;
                playSettings.Loudness = (60.0f * (Loudness / 100.0f)) - 48.0f;  // Convert percentage to decibels (dB)

                _playHandle = _audio.PlaySoundOnComponent(LoopingSound, playSettings);
            }
            // Else if a sound is playing, stop it
            else
            {
                if (_playHandle.IsPlaying())
                    _playHandle.Stop();

                _playHandle = null;
            }
        });
    }
}
```

As you can see from the above sample, play handles are returned by the sound play interfaces and they can
be used to control or manipulate a previously played sound.

For the complete set of functionaly related to sounds, check the API documentation
in your Sansar installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\AudioComponent.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\PlayHandle.html`

Also check the `PlaySound` and `PlaySoundAtPosition` functions on these classes:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\AgentPrivate.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ScenePrivate.html`


### Audio play settings

In the above examples we start with the `PlayOnce` or `Looped` settings and then adjusted the other attributes
as needed.  This is the recommended way to set up these audio calls.

Note that the `Loudness` setting is expected to be in decibels (dB) but most non-sound designers prefer to work
in percentages so we end up doing a little math to convert from percentages to dB for the play settings.
Here are some reference functions to do this conversion:

```c#
float LoudnessPercentToDb(float loudnessPercent)
{
    loudnessPercent = Math.Min(Math.Max(loudnessPercent, 0.0f), 100.0f);
    return 60.0f * (loudnessPercent / 100.0f) - 48.0f;
}

float LoudnessDbToPercent(float loudnessDb)
{
    float percent = (loudnessDb + 48.0f) * 100.0f / 60.0f;
    return Math.Min(Math.Max(percent, 0.0f), 100.0f);
}
```

The complete documentation can be found here:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\PlaySettings.html`


## How to control the media source

Like sounds, the media source for a scene can be controlled globally or on a per-user basis.  Scenes only
support a single media source though, and there is no functionality to save off a screenshot or otherwise
freeze a media source to an object.  When the media source updates, all surfaces that use the media source
material will update to reflect the new data.

The main two interfaces that change the media source can be accessed like so:

```c#
ScenePrivate.OverrideMediaSource("https://www.sansar.com/");  // scene-wide, all users will see this
agent.OverrideMediaSource("https://atlas.sansar.com/experiences/sansar-studios/");  // only for this agent
```

Here is a sample script to change the media source for a specific user when they interact with the object:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class PrivateMediaSourceScript : SceneObjectScript
{
    [DefaultValue("https://www.sansar.com/")]
    public string PublicMedia;

    [DefaultValue("https://atlas.sansar.com/experiences/sansar-studios/")]
    public string PrivateMedia;

    public override void Init()
    {
        // Set the public media source for the scene
        ScenePrivate.OverrideMediaSource(PublicMedia);

        // Override the media source to the private media source for each user that clicks on this object

        ObjectPrivate.AddInteractionData addData = (ObjectPrivate.AddInteractionData) WaitFor(ObjectPrivate.AddInteraction, "Show media", true);

        addData.Interaction.Subscribe((InteractionData data) =>
        {
            AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);

            if (agent != null)
            {
                agent.OverrideMediaSource(PrivateMedia);
            }
        });
    }
}
```

The complete reference for these functions can be found on these classes:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\AgentPrivate.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ScenePrivate.html`


## How to implement chat commands

Sometimes it can be handy to set up scripts to respond to chat commands.  Scripts can do this by 
subscribing to the default chat channel for the scene and parsing the messages, like so:

```c#
ScenePrivate.Chat.Subscribe(Chat.DefaultChannel, (ChatData data) => {
    if (data.Message == "hello script")
        ScenePrivate.Chat.MessageAllUsers("hi!");
});
```

Depending on the type of commands and control being put into chat commands, it is often a good
idea to put some level of restriction based on who sent the command.  So for example here is a
script that changes world gravity to a low value when the scene owner types "/setlowgrav" but
will ignore that message from any other user.

```c#
using Sansar.Script;
using Sansar.Simulation;

public class LowGravityChatCommandScript : SceneObjectScript
{
    public override void Init()
    {
        ScenePrivate.Chat.Subscribe(Chat.DefaultChannel, (ChatData data) => 
        {
            if (data.Message == "/setlowgrav")
            {
                AgentPrivate agent = ScenePrivate.FindAgent(data.SourceId);

                // If the agent is the scene owner
                if ((agent != null) && (agent.AgentInfo.AvatarUuid == ScenePrivate.SceneInfo.AvatarUuid))
                {
                    // Set the gravity to 15% of earth gravity
                    ScenePrivate.SetGravity(0.15f * 9.81f);

                    // Send a private acknowledgement message back to the user
                    agent.SendChat("You set low gravity!");
                }
            }
        });
    }
}
```

The complete reference for the chat system can be found here:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\Chat.html`


## How to put multiple scripts together into a single inventory item

As scene and script complexity grows, it is often handy to be able to share code between multiple
scripts or to define common interfaces for inter-script communication.  Or it can just be a convenient way
for projects to store all of their scripts within a single inventory resource.

Script resources that contain multiple scripts are referred to as "Script Assemblies" and there are two
ways to make them.

The easiest way is to define a namespace in your file and then to declare multiple classes within that
namespace:

```c#
using Sansar.Script;
using Sansar.Simulation;

namespace MyCustomNamespace
{
    public class Script1 : SceneObjectScript
    {
        public override void Init() {}
    }

    public class Script2 : SceneObjectScript
    {
        public override void Init() {}
    }

    // etc.
}
```

The import process for this script remains the same.  When the script is assigned to an object in the
scene, there will be an additional UI element to choose either `Script1` or `Script2` from the namespace.

If you prefer to distribute your scripts across multiple files then the setup will be a little different.
The classes will still need to be within your custom namespace but each one can sit in a separate file.
For this same example code let's say we define `Script1.cs` and `Script2.cs` in the same directory.
In order to import them as one into Sansar, a small JSON project file is required.  

The `MyCustomNamespace.json` file contents would be as follows:

```json
{
  "source": [
    "Script1.cs",
    "Script2.cs"
  ]
}
```

Importing this script assembly is then done by importing the JSON file instead of the C# files.

There are a few C# properties to improve the usage of a script assembly.  Namely you can override the
auto-generated name (usually to remove the namespace) and set the default class.  So for example if
you wanted `Script2` to be the default script selected when this assembly is put on an object and you
wanted to call it "Master Control Script" instead of "Script2", you would define it like so:

```c#
using Sansar.Script;
using Sansar.Simulation;

namespace MyCustomNamespace
{
    [Tooltip("This is the master control script.")]
    [DisplayName("Master Control Script")]
    [DefaultScript]
    public class Script2 : SceneObjectScript
    {
        public override void Init() {}
    }
}
```

Another way to shorten the name that shows up in the editor by just removing the namespace is:

```c#
namespace MyCustomNamespace
{
    [DisplayName(nameof(Script1))]
    public class Script1 : SceneObjectScript
    {
        public override void Init() {}
    }
}
```


## How to send and receive messages between scripts

Scripts can communicate with other scripts using messages.  These messages are broadcast throughout the
scene and can also include additional data.  This can be a good way to distribute scripting functionality
within your own scripts, but also to communicate with scripts written by other developers.  In addition,
it is the main mechanism that the so-called "Simple Scripts" use for communication.  
See the below section on simple script messaging for more information about this.

The two pieces to this are sending and receiving script messages, which is done through these interfaces:

```c#
PostScriptEvent("my_event");  // send "my_event"
SubscribeToScriptEvent("my_event", (ScriptEventData data) => {});  // listen for "my_event"
```

A pair of script instances could use this event to make a button that turns a light off:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

namespace MessagingScripts
{
    public class SendMessageScript : SceneObjectScript
    {
        public override void Init()
        {
            ObjectPrivate.AddInteractionData addData = (ObjectPrivate.AddInteractionData)WaitFor(ObjectPrivate.AddInteraction, "Turn off", true);

            addData.Interaction.Subscribe((InteractionData data) =>
            {
                // Send the "button_pressed" message
                PostScriptEvent("button_pressed");
            });
        }
    }

    public class ReceiveMessageScript : SceneObjectScript
    {
        private LightComponent _light;

        public override void Init()
        {
            if (!ObjectPrivate.TryGetFirstComponent(out _light))
                Log.Write("ReceiveMessageScript couldn't find light!");
            else if (!_light.IsScriptable)
                Log.Write("ReceiveMessageScript couldn't find scriptable light!'");
            else
            {
                // Listen for the "button_pressed" message
                SubscribeToScriptEvent("button_pressed", (ScriptEventData data) =>
                {
                    // Turn off the light
                    _light.SetColorAndIntensity(Color.Black, 0.0f);
                });
            }
        }
    }
}
```

Note also that declaring the namespace allows the script to include more than one class which is a nice
way to package up interdependent scripts into a single script assembly.


## How to connect your scripts to simple scripts

Messaging to and from "Simple Scripts" is exactly the same as messaging within your own scripts with the
added requirement of a specific data payload interface.  Simple scripts rely on this data payload to 
extract and reference specific objects and agents.

The interface of incoming simple script message data payloads is as follows:

```c#
public interface ISimpleData
{
    AgentInfo AgentInfo { get; }
    ObjectId ObjectId { get; }
    ObjectId SourceObjectId { get; }

    // Extra data
    Reflective ExtraData { get; }
}
```

The `AgentInfo` data conveys the agent that triggered this message.  Note that this is not always set
since some messages originate from objects that might not have been triggered by avatar interactions,
such as a timer or an object entering a trigger volume.

The `ObjectId` data is reliably set and contains the Id of the object that triggered the message.  
For example in a trigger volume collision, the Id of the object that entered the trigger volume would be
in this field.

The `SourceObjectId` data comes from the object that sent this message.  In the same trigger volume
example above, this would be the Id of the trigger volume itself.

Here is an example that writes to chat when the "on" message is sent from a simple script:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class SimpleListenerScript : SceneObjectScript
{
    public interface ISimpleData
    {
        AgentInfo AgentInfo { get; }
        ObjectId ObjectId { get; }
        ObjectId SourceObjectId { get; }

        // Extra data
        Reflective ExtraData { get; }
    }

    public override void Init()
    {
        // Listen for the 'on' message
        SubscribeToScriptEvent("on", (ScriptEventData data) =>
        {
            ISimpleData idata = data.Data.AsInterface<ISimpleData>();
            if (idata == null)
            {
                ScenePrivate.Chat.MessageAllUsers("The 'on' message does not have a simple script payload!");
            }
            else
            {
                ObjectPrivate obj = ScenePrivate.FindObject(idata.ObjectId);
                ScenePrivate.Chat.MessageAllUsers("The 'on' message simple script payload came from " + obj.Name);
            }
        });
    }
}
```

Sending a message back to simple scripts requires the creation of a payload that supports that same interface.
One way to do this is to use that exact interface mapped into a `SimpleData` class:

```c#
using Sansar.Script;
using Sansar.Simulation;

public class SimpleSenderScript : SceneObjectScript
{
    public interface ISimpleData
    {
        AgentInfo AgentInfo { get; }
        ObjectId ObjectId { get; }
        ObjectId SourceObjectId { get; }

        // Extra data
        Reflective ExtraData { get; }
    }

    public class SimpleData : Reflective, ISimpleData
    {
        public SimpleData(ScriptBase script) { ExtraData = script; }
        public AgentInfo AgentInfo { get; set; }
        public ObjectId ObjectId { get; set; }
        public ObjectId SourceObjectId { get; set; }

        public Reflective ExtraData { get; }
    }

    public override void Init()
    {
        ObjectPrivate.AddInteractionData addData = (ObjectPrivate.AddInteractionData)WaitFor(ObjectPrivate.AddInteraction, "Turn on", true);

        addData.Interaction.Subscribe((InteractionData data) =>
        {
            // Create the simple script message data payload
            SimpleData sd = new SimpleData(this);
            sd.ObjectId = ObjectPrivate.ObjectId;
            sd.SourceObjectId = ObjectPrivate.ObjectId;

            // Include the agent info for the avatar that triggered this event
            AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);
            if (agent != null)
            {
                sd.AgentInfo = agent.AgentInfo;
                sd.ObjectId = agent.AgentInfo.ObjectId;
            }

            // Send the "on" message with the SimpleData payload
            PostScriptEvent("on", sd);
        });
    }
}
```

The `Reflective` type above is a base interface that exists just for the purposes of being able to use a
common base type when communicating between scripts and interfaces.  We'll be using it more in the 
sections about working with other scripts in the scene so for now just know that it exists and plays a
role in inter-script communication.

The implementation of the simple script data payload interface and all of the other simple script helper 
functions can be found in your Sansar client installation:
* `C:\Program Files\Sansar\Client\ScriptApi\Examples\ScriptLibrary\LibraryBase.cs`


## How to find other scripts in the scene

The name of the mechanism within Sansar that allows one script to find another script within a scene is
`Reflective`.  Once a script interface is located, the calling script can make direct function calls into
the receiving script without the need to send messages back and forth.  It also allows for the 
distribution of properties across multiple script components and can be easier for iteration.

Doing this requires a two-part setup, namely the receiving script needs to register itself and the calling
script needs to know how to find the receiver.

On the receiving side, the class needs to register the reflective interface.  This is done using the 
`[RegisterReflective]` attribute like so:

```c#
using Sansar.Script;
using Sansar.Simulation;

[RegisterReflective]
public class ReflectiveReceiverScript : SceneObjectScript
{
    public Interaction Button;

    public override void Init() {}

    public void SetButtonEnabled(bool enabled)
    {
        Button.SetEnabled(enabled);
    }
}
```

Then on the calling side we query the reflective interfaces to try to find one that matches what we are
looking for.  This could be done like so:


```c#
using Sansar.Script;
using Sansar.Simulation;
using System.Linq;

public class ReflectiveCallerScript : SceneObjectScript
{
    public interface IButton { void SetButtonEnabled(bool enabled); }

    public override void Init()
    {
        IButton[] buttons = ScenePrivate.FindReflective<IButton>("ReflectiveReceiverScript").ToArray();
        foreach (IButton b in buttons)
        {
            b.SetButtonEnabled(false);
        }
    }
}
```

Note the coupling of `[RegisterReflective]` with `FindReflective`.  This can be done across script and
script assembly bounds and is very open ended.  In fact any script with the matching name that has the
defined interface or a superset of the defined interface will be located.

If the receiver script is within the same C# file or within the same script assembly, there is no need
to declare a special interface and the class type and name can be used directly.  In this case it would
be:

```c#
var buttons = ScenePrivate.FindReflective<ReflectiveReceiverScript>("ReflectiveReceiverScript").ToArray();
```

The `FindReflective` function reference can be found here:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ScenePrivate.html`


## How to find scripts on an object

In addition to the above scene-wide interface search, `ObjectPrivate` has a `FindScripts` function.
It functions similarly and can be acccessed like so:

```c#
var buttons = ObjectPrivate.FindScripts<ReflectiveReceiverScript>().ToArray();
```

This object version can be used for more directed operations to work within a limited scope of scripts
rather than across an entire scene.

There is a variant that allows the class name to be specified as well.  See the API reference here for
more details:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ObjectPrivate.html`


## How to make rest API calls from script

The scripting system can communicate directly with non-Sansar servers using HTTP requests.  This
supports `DELETE`, `GET`, `HEAD, `PATCH`, `POST` and `PUT` commands.  Although the use of HTTP 
requests can greatly increase the functionality available in your experiences, Sansar does not
provide any rest API endpoints intended for consumer storage of data.  Storage of scores, progress 
or other data will require your own server management.

The basic structure of an HTTP request in script is as follows:

```c#
void GetData()
{
    // Set up the request type
    HttpRequestOptions options = new HttpRequestOptions();
    options.Method = HttpRequestMethod.GET;

    // Fill in some parameters, in this case the sansar URI
    options.Parameters = new Dictionary<string, string>()
    {
        { "sansar_uri", ScenePrivate.SceneInfo.SansarUri },
    };

    // Make the request
    ScenePrivate.HttpClient.Request("http://api.my.com/api/v1/getdata",
                                    options, 
                                    (HttpClient.RequestData data) =>
    {
        // Check for success
        if (data.Success && data.Response.Status == 200)
        {
            // Parse data.Response.Body
        }
        else
        {
            Log.Write(LogLevel.Error, "HTTP request failed!");
        }
    });
}
```

Another facet of the API that is useful to enable HTTP in scripts is the `Sansar.Utility`
namespace which includes JSON serialization helpers.  This can be done via general purpose types:

```c#
var jsonData = WaitFor(JsonSerializer.Deserialize<Dictionary<string, int>>, data.Response.Body) as JsonSerializationData<Dictionary<string, int>>;
Dictionary<string, int> jsonDict = jsonData.Object;
```

Or by declaring classes that match the structure of the return data.

```c#
public class ItemDefinition
{
    public string Id;
    public string Title;
    // etc.
}

public class InventoryData
{
    public List<ItemDefinition> Data;
}

// code in some HTTP request return Action
var inventoryJson = WaitFor(JsonSerializer.Deserialize<InventoryData>, data.Response.Body) as JsonSerializationData<InventoryData>;
InventoryData inventoryData = inventoryJson.Object;
```

Note in this case that the names of the member variables of the classes passed to the JSON
serializer need to match the names of the keys in the JSON data.

If you wanted to combine storage and retrieval into a single script to track the visitors to your
scene, for example, you might do something like the following:

```c#
using Sansar.Script;
using Sansar.Simulation;
using Sansar.Utility;
using System;
using System.Collections.Generic;

public class HTTPVisitTrackerScript : SceneObjectScript
{
    [DefaultValue("https://api.my.com/api/v1/sansar_visitor_tracking")]
    public readonly string Endpoint;

    public override void Init()
    {
        if (!ScenePrivate.HttpClient.IsValid)
        {
            // In practice this should never happen but just in case...
            Log.Write(LogLevel.Error, "HTTP client invalid!  Visitor tracking disabled.");
            return;
        }

        // Track a visit when a user joins the scene
        ScenePrivate.User.Subscribe(User.AddUser, (UserData ud) => { TrackVisit(); });

        // Add a chat listener to retrieve visit count when "/visits" is written in chat
        ScenePrivate.Chat.Subscribe(Chat.DefaultChannel, (ChatData data) =>
        {
            if (data.Message == "/visits")
                GetVisits();
        });
    }

    void TrackVisit()
    {
        HttpRequestOptions options = new HttpRequestOptions();
        options.Method = HttpRequestMethod.POST;

        options.Parameters = new Dictionary<string, string>()
        {
            { "sansar_uri" , ScenePrivate.SceneInfo.SansarUri },
        };

        ScenePrivate.HttpClient.Request(Endpoint, options, (HttpClient.RequestData data) =>
        {
            if (!data.Success || data.Response.Status != 200)
            {
                ScenePrivate.Chat.MessageAllUsers("VisitTrackerScript TrackVisit: Error");
            }
        });
    }

    void GetVisits()
    {
        HttpRequestOptions options = new HttpRequestOptions();
        options.Method = HttpRequestMethod.GET;

        options.Parameters = new Dictionary<string, string>()
        {
            { "sansar_uri" , ScenePrivate.SceneInfo.SansarUri },
        };

        ScenePrivate.HttpClient.Request(Endpoint, options, (HttpClient.RequestData data) =>
        {
            if (data.Success && data.Response.Status == 200)
            {
                Dictionary<string, int> jsonData = ((JsonSerializationData<Dictionary<string, int>>)(WaitFor(JsonSerializer.Deserialize<Dictionary<string, int>>, data.Response.Body))).Object;

                // print returned "visits" count to chat
                ScenePrivate.Chat.MessageAllUsers("Total visits: " + jsonData["visits"]);
            }
            else
            {
                ScenePrivate.Chat.MessageAllUsers("VisitTrackerScript GetVisits: Error");
            }
        });
    }
}
```

The documentation around HTTP and JSON use in Sansar can be found on a few different pages:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\HttpClient.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\HttpRequestMethod.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\HttpRequestOptions.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ScenePrivate.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Utility\JsonSerializer.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Utility\JsonSerializationData.html`


## How to parse JSON

Many rest API calls return JSON blobs of data which can be parsed through JSON deserialization.
The above example shows how to use a more generic structure in a simple case but it can be tricky
to get it to work properly for a more complex example.

Setting up the data structures can be finicky so we will look carefully at this 
[fortune cookie heroku app](http://fortunecookieapi.herokuapp.com/)
and set up a sample script to GET a fortune and parse it into the constituent pieces.
Specifically we will use a GET request on the `/v1/cookie` endpoint like so:

```c#
private readonly string url = "http://fortunecookieapi.herokuapp.com/v1/cookie";

void GetFortune()
{
    // Set up an HTTP GET request
    HttpRequestOptions options = new HttpRequestOptions();
    options.Method = HttpRequestMethod.GET;
    options.Parameters = new Dictionary<string, string>(){ { "limit", "1" } };  // unnecessary since the default is 1 but here as reference

    // Do the HTTP request
    ScenePrivate.HttpClient.Request(url, options, (HttpClient.RequestData data) =>
    {
        // Check for success
        if (data.Success && data.Response.Status == 200)
        {
            // Display the entire body of unparsed JSON in the debug log for reference
            Log.Write($"Unparsed JSON:\n{data.Response.Body}\n");
        }
    }
}
```

The JSON blob that is returned will look something like the following:

```json
[
  {
    "fortune":{"message":"Too many cooks spoil the broth.","id":"5403c81dc2fea4020029abe0"},
    "lesson": {"english":"100,000","chinese":"十万","pronunciation":"shí-wàn","id":"5404c5404cad2502004dee54"},
    "lotto":  {"id":"000400040023003100080005","numbers":[4,4,23,31,8,5]}
  }
]
```

Note this is a little unusual for the outer block to be an array (with `[]`) rather than a dictionary
so this example has an additional complication to work through in the code.

To take advantage of the JSON deserialization features you must declare classes that mirror the structure of this JSON:

```c#
public class FortuneData { public string message; public string id; }
public class LessonData { public string english; public string chinese; public string pronunciation; public string id; }
public class LottoData { public string id; public List<int> numbers; }

public class CookieData { public FortuneData fortune; public LessonData lesson; public LottoData lotto; }
```

Note the names of the member variables need to match the names of the dictionary key entries in the JSON data itself.

Lastly to parse this, we have to remember that the outer block is an array so the code will look like so:

```c#
var cookiesDefinition = WaitFor(JsonSerializer.Deserialize<CookieData[]>, data.Response.Body) as JsonSerializationData<CookieData[]>;
CookieData[] cookiesData = cookiesDefinition.Object;
```

Once we have it in this form, the remainder of the data can be accessed like any other object.

Here is a full sample that will repond to a `fortune` or `/fortune` chat command by making a GET request
and printing the relevant info to nearby chat.

```c#
// © 2019 Linden Research, Inc.

// Special thanks to NyushaZoryAna for making me aware of the rest service, the idea for this sample and help making this functional.

using Sansar.Script;
using Sansar.Simulation;
using Sansar.Utility;
using System;
using System.Collections.Generic;

public class HTTPFortuneJSONScript : SceneObjectScript
{
    private readonly string url = "http://fortunecookieapi.herokuapp.com/v1/cookie";

    // The above url will return a JSON blob like this:
    //
    //[
    // {
    //  "fortune":{"message":"Too many cooks spoil the broth.","id":"5403c81dc2fea4020029abe0"},
    //  "lesson": {"english":"100,000","chinese":"十万","pronunciation":"shí-wàn","id":"5404c5404cad2502004dee54"},
    //  "lotto":  {"id":"000400040023003100080005","numbers":[4,4,23,31,8,5]}
    // }
    //]
    //
    // NOTE: This is a little unusual with the outer array block which makes it slightly tricky to parse.

    // Define public classes with public member variables to mirror the JSON data layout
    public class FortuneData { public string message; public string id; }
    public class LessonData { public string english; public string chinese; public string pronunciation; public string id; }
    public class LottoData { public string id; public List<int> numbers; }
    public class CookieData { public FortuneData fortune; public LessonData lesson; public LottoData lotto; }

    public override void Init()
    {
        if (!ScenePrivate.HttpClient.IsValid)
        {
            // In practice this should never happen but just in case...
            Log.Write(LogLevel.Error, "HTTP client invalid!  Fortune cookie retrieval disabled.");
            return;
        }

        // Add a chat listener to retrieve visit count when "fortune" or "/fortune" is written in chat
        ScenePrivate.Chat.Subscribe(Chat.DefaultChannel, (ChatData data) =>
        {
            if ((data.Message == "fortune") || (data.Message == "/fortune"))
                GetFortune();
        });
    }

    void GetFortune()
    {
        // Set up an HTTP GET request
        HttpRequestOptions options = new HttpRequestOptions();
        options.Method = HttpRequestMethod.GET;
        options.Parameters = new Dictionary<string, string>(){ { "limit", "1" } };  // unnecessary since the default is 1 but here as reference

        // Do the HTTP request
        ScenePrivate.HttpClient.Request(url, options, (HttpClient.RequestData data) =>
        {
            // Check for success
            if (data.Success && data.Response.Status == 200)
            {
                // Display the entire body of unparsed JSON in the debug log for reference
                Log.Write($"Unparsed JSON:\n{data.Response.Body}\n");

                // Parse the data into an array of CookieData
                var cookiesDefinition = WaitFor(JsonSerializer.Deserialize<CookieData[]>, data.Response.Body) as JsonSerializationData<CookieData[]>;
                CookieData[] cookiesData = cookiesDefinition.Object;

                // Print the relevant fields to chat

                var fortune = cookiesData[0].fortune;
                ScenePrivate.Chat.MessageAllUsers($"Your fortune: {fortune.message}");

                var lesson = cookiesData[0].lesson;
                ScenePrivate.Chat.MessageAllUsers($"Chinese lesson: {lesson.chinese} ({lesson.pronunciation}) means {lesson.english} in English");

                var lotto = cookiesData[0].lotto;
                ScenePrivate.Chat.MessageAllUsers($"Lotto numbers: {string.Join(", ", lotto.numbers)}");
            }
            else
            {
                ScenePrivate.Chat.MessageAllUsers("HTTPFortuneJSONScript GetFortune: Error");
            }
        });
    }
}
```

The documentation around HTTP and JSON use in Sansar can be found on a few different pages:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\HttpClient.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\HttpRequestMethod.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\HttpRequestOptions.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ScenePrivate.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Utility\JsonSerializer.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Utility\JsonSerializationData.html`


## How to listen for trigger volume events

Trigger volumes are special physics objects that can detect when other objects or avatars
pass through them.  These are very handy for auto-opening doors, general detection of players
or a variety of other situations.

Find the "Trigger Volume" asset in the System section of the editor inventory and add it to the
scene.  Then add a script to the trigger volume to detect objects:

```c#
using Sansar;
using Sansar.Script;
using Sansar.Simulation;

public class TriggerVolumeScript : SceneObjectScript
{
    public override void Init()
    {
        RigidBodyComponent rigidBody;

        if (ObjectPrivate.TryGetFirstComponent(out rigidBody) && rigidBody.IsTriggerVolume())
            rigidBody.Subscribe(CollisionEventType.Trigger, OnTrigger);
        else
            Log.Write(LogLevel.Warning, "TriggerVolumeScript not running on a trigger volume!");
    }

    void OnTrigger(CollisionData data)
    {
        AgentPrivate agent = ScenePrivate.FindAgent(data.HitComponentId.ObjectId);

        if (agent != null)
        {
            if (data.Phase == CollisionEventPhase.TriggerEnter)
                agent.SendChat("Agent entered trigger volume!");
            else if (data.Phase == CollisionEventPhase.TriggerExit)
                agent.SendChat("Agent exited trigger volume!");
        }
    }
}
```

The documentation around this can be found here:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\CollisionData.html`
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\RigidBodyComponent.html`


## How to check the physics world with raycasts and shapecasts

There are a variety of raycast and shape cast options available in the scene private API.  These
can be used to query the physics objects in the scene for navigation of script controlled NPC's or
whatever else.

Basic use is fairly straightforward but sometimes it can be tricky to figure out exactly what
types of objects have been hit.  Here is an example that sorts through all of the hits of a
sphere cast to apply a force to the dynamic rigidbodies:

```c#
void PushObjects()
{
    float radius = 5.0f;
    float distance = 10.0f;
    Vector pos = ObjectPrivate.Position;
    RayCastHit[] castHits = ScenePrivate.CastSphere(radius, pos, pos + Vector.Up * distance, ScenePrivate.MaximumCastRayResults);

    for (int i = 0; i < castHits.Length; i++)
    {
        // Ignore agents
        var agent = ScenePrivate.FindAgent(castHits[i].ComponentId.ObjectId);
        if (agent != null)
            continue;

        // Ignore other things, whatever these might be
        var obj = ScenePrivate.FindObject(castHits[i].ComponentId.ObjectId);
        if (obj == null)
            continue;

        // Apply a force to push all of the dynamic objects up
        RigidBodyComponent rb = obj.GetComponent(ComponentType.RigidBodyComponent, 0) as RigidBodyComponent;
        if ((rb != null) && (rb.GetMotionType() == RigidBodyMotionType.MotionTypeDynamic))
            rb.AddLinearImpulse(Vector.Up * 100.0f);
    }
}
```

Check the `Cast[Shape]` and `Get[Shape]ClosestPoints` functions for more ways to query the
physics world:
* `C:\Program Files\Sansar\Client\ScriptApi\Documentation\Sansar.Simulation\ScenePrivate.html`


# Gotchas

The Sansar scripting system was designed with different constraints than most other game engines
and as a result there are some quirks to using the API that will come as a surprise to most
programmers.


## Set functions and WaitFor

Most of the functions within the scripting API that set values on objects do not actually apply
the settings immediately.  This is for a variety of reasons from stability to optimization.  Some
physics properties will trigger a cascade of other computations when changed, for example, so the
system will wait until the sript system is done before applying any changes, in case the value is
modified multiple times within a single frame.  

Because of this, some code you might expect to work will most likely yield strange results.  Consider
this example on a rigidbody component:

```c#
WaitFor(rigidBody.SetMass, 5.0f);
rigidBody.SetMass(2.0f);
float mass = rigidBody.GetMass();
Log.Write($"Mass is: {mass}");
```

It is very likely that the printed mass in this case will actually be the previously assigned mass
of "5.0" although it is possible for it to be "2.0" instead, depending on what else is going on in
the scene.  If you have a variety of settings to assign, you probably don't want to put a `WaitFor`
around each of them as that will cause each setting to be assigned sequentially and wait for it to
be applied before continuing execution to the next line.


## Unhandled exceptions kill your script

Any script that triggers or throws an exception that is not caught by the script, will be killed
by the system.  This means that proper exception handling is required in order to make your scripts
robust.  This seems like a no-brainer for obvious exceptions but read on to learn about exceptions
that might creep into your code unexpectedly!


### Scripts can be preempted

In engines such as Unity, each script has an `Update` call that will be executed once per frame.
Sansar has no equivalent to this.  You can set up a coroutine with a small `Wait` to do a periodic
update but be aware that each individual script could be preempted at nearly any time.

This means that it is nearly impossible to write scripts that won't ever trigger exceptions.

For example this code could trigger an exception:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);
    if (agent != null)
        agent.SendChat("Hello from script!");
});
```

It is very unlikely but not impossible for the agent to become invalid betwen the if check
and the inner portion of the if statement.  For that reason, to make this snippet more robust we
must expect an exception:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    AgentPrivate agent = ScenePrivate.FindAgent(data.AgentId);
    if (agent != null)
    {
        try
        {
            agent.SendChat("Hello from script!");
        }
        catch
        {
            Log.Write(LogLevel.Warning, "Unable to send chat to agent due to exception.");
        }
    }
});
```

This is particularly important when dealing with agents as they are amongst the most unpredictable
objects in the scene, since a user can disconnect, crash, logout or move on to another experience
at any time.

There is an `agent.IsValid` check that can also be used to check on the validity of the agent, but
since it doesn't help with the script pre-empting issue it isn't terribly useful in practice.


### Throttle exceptions

Another common gotcha is that not everything can be executed as quickly as you might like.  When
your script attempts to exceed the maximum rate that some function calls are limited to, it will
throw a throttle exception.  Proper handling is a requirement around these types of operations.

For example consider a button to change the media source to another channel in an array of 
youtube channel urls:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    _channel = (_channel + 1) % _youtubeChannels.Length;  // increment to next channel
    ScenePrivate.OverrideMediaSource(_youtubeChannels[_channel]);
});
```

Since we don't know how quickly or slowly a user might press this button we will need to consider
if it is being pressed faster than the media source throttle rate.  If we do not handle this
exception the script will be killed and the button will stop functioning.  So a naive solution
might be:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    try
    {
        _channel = (_channel + 1) % _youtubeChannels.Length;  // increment to next channel
        ScenePrivate.OverrideMediaSource(_youtubeChannels[_channel]);
    }
    catch (ThrottleException)
    {
        Log.Write("Channel not changed due to throttle limiting.");
    }
});
```

The unfortunate part about this workaround would be that any throttled channel changes would skip
over that youtube channel.  So if we wanted to make this fully functional without skipping over
anything in the list, we might end up with something like:

```c#
InteractionProperty.Subscribe((InteractionData data) =>
{
    // Disable the button until we successfully update the channel
    InteractionProperty.SetEnabled(false);

    _channel = (_channel + 1) % _youtubeChannels.Length;  // increment to next channel

    bool success = false;
    while (!success)
    {
        try
        {
            // Try to change the channel
            ScenePrivate.OverrideMediaSource(_youtubeChannels[_channel]);
            success = true;
        }
        catch (ThrottleException)
        {
            // Wait a second and try again
            Wait(TimeSpan.FromSeconds(1.0));
        }
    }

    // The channel has been updated, re-enable the button
    InteractionProperty.SetEnabled(true);
});
```

The complete list of functions that are throttled and will throw a `ThrottleException` is:

Function Name | Throttle Rate
--------------|--------------
AgentPrivate.SendChat | 64 calls per 2 seconds
AgentPrivate.OverrideAudioStream | 5 calls per 10 seconds
AgentPrivate.OverrideMediaSource | 5 calls per 10 seconds
AgentPrivate.PerformMediaAction | 5 calls per 10 seconds
AgentPublic.SendChat | 32 calls per 2 seconds
ScenePrivate.Chat.MessageAllUsers | 32 calls per 2 seconds
ScenePrivate.CreateCluster | 100 calls per second
ScenePrivate.HttpClient.Request | 10 calls per second
ScenePrivate.OverrideAudioStream | 5 calls per 10 seconds
ScenePrivate.OverrideMediaSource | 5 calls per 10 seconds
ScenePrivate.PerformMediaAction | 5 calls per 10 seconds


### Logging is throttled too

When scripts write too much text into the debug log, log messaging will be throttled.
However, this does not throw a `ThrottleException` and instead will result in a warning
message in the debug console letting you know the logging has been throttled.

For this reason, it is often a good idea to clean up scripts after they are functional
to save room in the debug console for new script development.  Or consider putting a 
"Verbose" property on your script to enable and disable logging so you can choose what
script messages to see.

