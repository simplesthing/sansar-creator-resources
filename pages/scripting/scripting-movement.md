---
title:  "Scripting Movement"
summary: "How to script movement in Sansar."
category: scripting
tags: [ getting_started, quaternions ]
sidebar: sidebar
permalink: scripting-movement.html
topnav: topnav
author: Binah
last_updated: August 27,, 2019
---

So you want to move stuff. Lets define what stuff can be moved, there are two types of objects in Sansar that can be scripted to move, physical objects and non physical objects. 

## Physical objects

Physical objects are are objects that have collision, you will know if they have collision by looking at the object `Properties`, you will see a section for `Volume` as well as setting for `Motion Type`, `Friction` and `Bounce`

![Object structure]({{ site.baseurl }}/images/movement/volume-properties.png)


### Motion Type Property

The `motion type` property is extremely important when it comes to physics objects and it greatly affects how the object behaves and what can be done to the object from the script API.

The `Static` motion type indicates that the object will never be moving. This is considered the most restrictive motion type. In fact static objects built into the scene are assumed never to move and are potentially optimized by the build process, so script manipulation of static objects is not possible.

The `Keyframed` motion type indicates that the object will only move when explicitly moved from script. This is the next most restrictive motion type. When keyframed objects are moved, they will not stop or otherwise be affected by any other collisions. They will simply move through everything and push avatars and dynamic objects out of the way.

The `Dynamic` motion type indicates that the object will be subject to gravity and other phsyical interactions. This is the least restrictive motion type. Dynamic objects will fall, roll and slide depending on what forces get applied to them in the scene. They will collide with static and keyframed objects and avatars.

Motion types can be changed from script but only to a more restrictive motion type than the initial import or scene settings allow. So for example, an object imported as `Dynamic` can be set to `Keyframed` from script but an object imported as `Static` cannot be set to `Keyframed` or `Dynamic`.

<!-- ![Object structure]({{ site.baseurl }}/images/movement/motion-type-keyframe.png) -->

## Non physical objects

Objects that have no collision are considered non-physical objects in Sansar. These can be moved with the Mover API if they are configured to be allowed to move. Much like `Static` objects above, objects that are not configured for movement are assumed never to move and are potentially optimized by the build process.

To configure an object for movement, set the `Movable From Script` and the `IsScriptable` attributes set to `On`.

![Object structure]({{ site.baseurl }}/images/movement/non-physical-settings.png)


## Mover API

The [`Mover API`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/Mover.html) can drive non-physical objects as well as physical objects that have their `Motion Type` set to `Keyframed` as well as  `Movable From Script` toggle `On`.

Properly configured objects can then be immediately moved using any of these functions, where `position` is a [`Sansar.Vector`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Vector.html) and `rotation` is a [`Sansar.Quaternion`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Quaternion.html).


    ObjectPrivate.Mover.AddMove(position, rotation);
    ObjectPrivate.Mover.AddTranslate(position);
    ObjectPrivate.Mover.AddRotate(rotation);

You may also use these interfaces to control the movement over time using additional parameters specifying length of time in seconds and [`MoveMode`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/MoveMode.html), `EaseIn`, `EaseOut`, `Linear` and `SmoothStep`.

    ObjectPrivate.Mover.AddMove(position, rotation, seconds, moveMode);
    ObjectPrivate.Mover.AddTranslate(position, seconds, moveMode);
    ObjectPrivate.Mover.AddRotate(rotation, seconds, moveMode);

Mover functions act like a queue and execute commands sequentially. In this way it is possible to make a simple behavior to move an object through a set of points as shown in the [Patrol Mover](https://github.com/lindenlab/sansar-script/blob/961122c8aac22206d615455be56e3d8430c19d58/Samples/PatrolMoverScript.cs) sample script snippet below.

    ObjectPrivate.Mover.AddTranslate(point1, 5.0, MoveMode.Linear);
    ObjectPrivate.Mover.AddTranslate(point2, 5.0, MoveMode.Linear);
    ObjectPrivate.Mover.AddTranslate(point3, 5.0, MoveMode.Linear);
    ObjectPrivate.Mover.AddTranslate(point4, 5.0, MoveMode.Linear);

Utility functions like `WaitFor`, [`AddPause`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/Mover.html#M:Sansar.Simulation.Mover.AddPause(System.Double)) and [`StopAndClear`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/Mover.html#M:Sansar.Simulation.Mover.StopAndClear()) can be used to control movement commands executions for finer control over movement. 


## Vectors

[`Sansar.Vector`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Vector.html) struct represents a three dimensional vector using the standard [Cartesian coordinate system](https://mathinsight.org/vectors_cartesian_coordinates_2d_3d#vector3D) of `X`,`Y`,`Z`. The Vector class provides three interfaces for supplying coordinate values but the most common is in the signature of `Vector (float X, float Y, float Z, float W)`. The `W` coordinate should not be needed in most scripting use cases, it's used to compute matrix mathematics and indicates a position or transform, it defaults to 0 and is assumed to be 0 for most operations if it is not specified, you can omit it entirely.

![3D Vector]({{ site.baseurl }}/images/movement/cartesian-coordinates.png)

Lets look at the [Patrol Mover](https://github.com/lindenlab/sansar-script/blob/961122c8aac22206d615455be56e3d8430c19d58/Samples/PatrolMoverScript.cs) sample script to see an example of how we can use vectors in our scripts to move an object along the vector axis'.

Start by creating a new scene and choosing the `Base Template Scene`. 

![Base Template Scene]({{ site.baseurl }}/images/movement/base-template-scene.png)

Notice the scene is blank with nothing but a spawn point. In the Settings for the spawn point set the position `X = 0`, `Y = -0.5`, `Z = 0` this will place the tip of the spawn point, making it look like an arrow pointing, at the coordinates `0,0,0`. From above it should look like a piece of graph paper where the grid lines indicate each square area of the `X` and `Y` axis'.

![Base Template Spawn]({{ site.baseurl }}/images/movement/base-spawn.png)

Next drag an object to animate into the scene, I am using a free [Wrasse Fish](https://store.sansar.com/listings/9e3ea0eb-bc71-44ae-8b3e-a5e7e382065f/wrasse-fish) non physical object in this example as it has a distinct head and tail which will be important in the next section when we discuss rotation. 

For now drag your object in front of the spawn point and set it coordinates at `Position` `X = 1`, `Y = 1`, `Z = 0` so that you are to the front right of the scene center. You'll also want to turn on the `Movable from Script` and `IsScriptable` toggles.

![Base Template Spawn]({{ site.baseurl }}/images/movement/fish-initial.png)

Import the [Patrol Mover](https://github.com/lindenlab/sansar-script/blob/961122c8aac22206d615455be56e3d8430c19d58/Samples/PatrolMoverScript.cs) sample script and attach it to the fish. It will provide four inputs to create an area to patrol. Lets keep it simple and input the coordinates for four points of a single cell square.

![Patrol Mover Settings]({{ site.baseurl }}/images/movement/patrol-settings.png)

Build and visit the scene to see the fish is indeed moving around the square, but fish don't move like that, so we're going to have add some rotations so that the fish swims in the direction its head is facing, like _irl_.

_Click to see video on YouTube_ : [![Fish video](https://img.youtube.com/vi/pEwJ2eF_zno/0.jpg)](https://www.youtube.com/watch?v=pEwJ2eF_zno)

Go back to your scene and remove the Patrol Mover script from the fish. Import the [Patrol Turn Mover](https://github.com/lindenlab/sansar-script/blob/master/Samples/PatrolTurnMoverScript.cs) sample script and add to the fish. 

Lets look at what makes this script different. For starters the public fields are expanded to allow more than a four point patrol if you desire. There is also a field called `WorldObjectForward` which is defining the vector to which the object faces forward, in our case our fish is facing forward to the left, which if you were to translate the fish from vector `0,0,0` forward by one unit it would be moving to vector `-1,0,0`.

        public List<Vector> PatrolPoints;

        [DefaultValue(1.0f)]
        public float MoveSpeed;

        [DefaultValue("<0,1,0>")]
        public Vector WorldObjectForward;

In the coroutine we see that a variable is set for the Next patrol vector called `toNext` and a rotation is calculated using the `WorldObjectForward` and the `toNext` vectors via a Quaternion method called [`ShortestRotation`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Quaternion.html#M:Sansar.Quaternion.ShortestRotation(Sansar.Vector@,Sansar.Vector@)) which as it's name states will generates the shortest rotation quaternion to rotate from one vector to another.

Input the following settings, build and visit.

![Patrol Turn Mover Settings]({{ site.baseurl }}/images/movement/patrol-turn-settings.png)

Did you see what I saw? The fish patrols the vectors but seems to disappear out of view for a section of the patrol and then reappear to continue its way.

_Click to see video on YouTube_ : [![Fish Turning video](https://img.youtube.com/vi/mysyNLPi_WE/0.jpg)](https://www.youtube.com/watch?v=mysyNLPi_WE)

Before we answer what happened to the fish I think we need to take a moment to talk about quaternions and then we can solve the mystery of the disappearing fish.

## Quaternions

[`Sansar.Quaternion`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Quaternion.html) struct represents a quaternion orientation. Quaternions are not a light subject this tutorial cannot provide a lesson on quaternion math but instead make you familiar with the concepts and the usage of the [`Sansar.Quaternion`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Quaternion.html) class and it's methods in Sansar scripting. 

The most common uses for your scripts will be to rotate an object from one point to another based on an angle. You might think to yourself, isn't rotation by angle enough, why complicate it with Quaternions? The answer has to do with optimizations and to avoid unexpected behaviors such as "Gimble Lock". 

Let's watch a quick [video](https://www.youtube.com/watch?v=Ocoibc7MoKg) describing at a high level why Quaternions are used.

And one more [video](https://www.youtube.com/watch?v=zc8b2Jo7mno) to visualize rotation with Euler Angles and the problem of Gimble Lock.

If you are looking for a lesson on quaternion math this [video](https://www.youtube.com/watch?v=3BR8tK-LuB0) does a great job explaining the math starting from how 2D complex numbers can be used to represent 2D rotation (and scale) in real space and why multiplying two 2D complex numbers together results in a 2D rotational transform, and then extending that logic to show how 4D numbers result in calculation of rotation in 3D space. 

Don't feel you have to understand the math to understand how to use the Quaternion class, its perfectly fine if you do not. The scripting API provides methods to convert angles to quaternions and use them for rotations using the Mover API. There are more resources for the curious below in [resources](#resources) section but for now we're going to move back into rotation of objects in Sansar Namespace and the mystery of the disappearing fish.

In my humble opinion the best way to gain insight to what is happening with your script is to add debug logs at each critical computation to inspect exactly what values are being used. This is what that looks like for the Patrol Turn Mover script.

     while (true)
        {
            // Calculate direction to next patrol point
            Vector toNext = PatrolPoints[next] - PatrolPoints[current];

            Log.Write("Calculate direction to next patrol point " + toNext);

            // Compute a world space rotation for this object to point at the next patrol point
            Quaternion rotation = Quaternion.ShortestRotation(WorldObjectForward, toNext.Normalized());

            Log.Write("Compute a world space rotation for this object to point at the next patrol point " + rotation);
            
            ObjectPrivate.Mover.AddRotate(rotation);  // Immediately turn to face

            // Compute the time based on the distance and move speed
            double moveTime = toNext.Length() / MoveSpeed;

            Log.Write("Compute the time based on the distance and move speed " + moveTime);

            // Move the object to the next patrol point
            WaitFor(ObjectPrivate.Mover.AddTranslate, PatrolPoints[next], moveTime, MoveMode.Linear);

            Log.Write("Move the object to the next patrol point " +  PatrolPoints[next]);

            // Increment to the next patrol point
            current = next;
            next = (next + 1) % PatrolPoints.Count;

        }

Now when I visit the scene and `ctrl + d` to open the debug panel I can see that the rotation value at the offending turn, outlined in red, has a vast difference in its value for the `rotation` than the others, outlined in yellow.

![Rotation Debug]({{ site.baseurl }}/images/movement/rotation-debug.jpg)
 
 I wasn't certain why this was happening and I asked one of the script team members what was going on, and it was discovered that the axis for the fish was situated lower to the floor in the fish giving its center an offset. When using [`ShortestRotation`](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar/Quaternion.html#M:Sansar.Quaternion.ShortestRotation(Sansar.Vector@,Sansar.Vector@)) method using only the two vectors can potentially give any spin, rotation around the axis in the direction it is facing, since it is only using two vectors it doesn't know what spin to use. So what happens then it is the fish's origin has an offset from its center which is then flipped upside down because that is computationally the shortest rotation between the two vectors, the fish continues to move underground out of view, and then flip back again to continue on its way. There might be more than one possible solution to this, fixing the center origin of the fish to remove the offset would likely be the easiest, but if that is not possible there are other ways to get the rotation value for the fish. 

**Update :**
This fish was further discussed in the August 30th event during a mob programming session where we modified the [MoverExample3:  aka, spinning scared robot](https://github.com/lindenlab/sansar-script/blob/master/Examples/MoverExample3.cs), an interaction script that makes a robot flee away and rotate randomly when you interact with it, to make the fish turn 90 degrees to the left and swim away from you. 

The first thing we did was to remove the `Spin` method and `SpinSpeed` property associated with it. Next we remove the fleeing away code at line 72:

    WaitFor(ObjectPrivate.Mover.AddTranslate, ObjectPrivate.Position + fromAgentToObject * FleeDistance, FleeSeconds, MoveMode.EaseOut);

And replace it with 

    Quaternion rot = ObjectPrivate.Rotation;
    Quaternion stepRot = Quaternion.FromEulerAngles(new Vector(0,0,(float)Math.PI/2f));
    rot *= stepRot;
    ObjectPrivate.Mover.AddRotate(rot);
    ObjectPrivate.Mover.AddTranslate(ObjectPrivate.Position + Vector.ObjectLeft.Rotate(rot), 2.0, MoveMode.Linear);


Stepping through: 
- the first line obtains the fish's world rotation value
-  the second line obtains the 90 degree rotation of the Z axis, the axis which we wish to rotate the fish on
- the third line multiplies the two quaternion values to obtain the final rotation value 
- the fourth line applies the rotation using the Mover API
- the fifth line translates the fish forward using the Object's position plus the left rotation quaternion which we have calculated, at a time of 2 seconds with a linear move mode.

_Special thanks to **MadEye** and **EvoAvo** for coming up with the solution to this exercise, final script can be found [here](https://github.com/lindenlab/sansar-script/blob/master/Users/binah/movement/Mover3b.cs)_


### Resources

A handy site for visualizing/converting quaternions and euler angles [quaternions.online](https://quaternions.online/)

An in depth series of interactive video lessons called [Visualizing Quaternions](https://eater.net/quaternions)

[OpenGL tutorial](http://www.opengl-tutorial.org/intermediate-tutorials/tutorial-17-quaternions/) on rotations.

