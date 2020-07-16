---
title:  "Access Control"
summary: "In this post we will be building a better mouse trap to catch and expel unwanted pests at our parties."
category: scripting
tags: [ event_hosting ]
sidebar: sidebar
permalink: access-controls.html
topnav: topnav
author: Binah
last_updated: November 7, 2019
---

In this post we will be building a better mouse trap to catch and expel unwanted pests at our parties.

The following assets are being used in this post:
- [Invisible Cube - Collision](https://store.sansar.com/listings/d6b7b9ed-e403-43a5-aebe-b362c73e52d9/invisible-cube---collision)
- [Access Control Script](https://github.com/lindenlab/sansar-script/blob/master/Users/binah/access-control/access-control.cs)

## Moderation Tools

Sansar offers a set of in client [moderation tools](https://help.sansar.com/hc/en-us/articles/360000825986-Moderation-tools-for-world-owners) that an owner of a scene can `%%kick` users that are behaving badly and add them to a banlist to keep them from returning to the scene. But what happens when a group of users flood into a party unexpectedly all at once and the scene owner has to copy all their names in two different admin tools, or what if the scene owner is not available to administer the commands? Getting creative with scripting you can enforce some rules and add some safety features to keep the peace.

## Amin privileges

By providing a script parameter for a comma separated list of administrators a scene owner can grant admin abilities to a known group of people at scene creation time. Additionally adding chat commands to add an admin, `/admin username`, and remove an admin, `/radmin username`, on the fly a scene owner can leave the party without worry for their guests.

## Voting system

By providing a way to enact a vote to ban a user through chat, `/vote username`. Two modes, either only admins can vote or in the case of no admins present in the scene anyone can call a vote and cast a vote.


## Persistence

You may be wondering how do we kick and ban via script? Technically you can't actually add a user to the ban list of the client moderation tools nor can you issue a `%%kick` command. But there are creative ways to eject unwanted Sansarians and make sure they stay out. A session script can keep record of both an admin list and banned list that would persist for that session. With the future promise of a persistent api just over the horizon it would be the next logical step to persist the lists, even share them with other scenes, and have that list persist between sessions. 

## Security Door

Most in real life places that wish to have control over who enters and leaves has some form of security barrier that you must qualify to pass through before you can join the party. We'll be doing the same.

Turn on the `Physics Visibility` for the scene.

![invisible cube]({{ site.baseurl }}/images/access-control/physics-visibility.png)

Then drag out the [Invisible Cube - Collision](https://store.sansar.com/listings/d6b7b9ed-e403-43a5-aebe-b362c73e52d9/invisible-cube---collision) and place it over your spawn point. You can turn off the `Physics Visibility` once it has been placed.

![physics visibility]({{ site.baseurl }}/images/access-control/invisible-cube.png)

I scaled it up by `2` for good measure, make sure `Has Collision` and `IsScriptable` are set to `On`.

![invisible cube]({{ site.baseurl }}/images/access-control/invisible-cube-props.png)

Build and test that you cannot get out of the invisible collision cube when you spawn into your scene.

Import [Access Control Script](https://github.com/lindenlab/sansar-script/tree/master/Users/binah) add to the cube and configure.

![access control script]({{ site.baseurl }}/images/access-control/import-script.png)

![access control script]({{ site.baseurl }}/images/access-control/access-control.png)

Setting the `Doors Open` will allow your scene to be freely visited, you can use chat command `/door closed` when you want to restrict entry. When the doors are closed all non admin entries will be held until an admin makes a decision to allow entry or to bannish.

![access control script]({{ site.baseurl }}/images/access-control/entry-prompt.png)

### Chat Commands

 `/help` for a list of the available chat commands

    [Script] Access Control Command usage:
    /help 
    /log 
    /reset 
    /door [open or close]
    /ban  [user-handle]
    /unban [user-handle]
    /admin [user-handle]
    /radmin [user-handle]
    /vote [user-handle]


 `/log` for a log of all lists and visitors

    [Script] There have been 1 visitors
    The doors are open: True
    Visitor list: 
    Name: Binah - Sansar
    - isAdmin: True
    Admin list: 
    - binah-sansar
    Banned list: 

  `/reset ` to reset a scene for everyone

  `/door open` to open the security gate, `/door close` to restrict access

  `/ban user-handle` to add user to ban list and expel

  `/unban user-handle` to remove from ban list

  `/admin user-handle` to add an admin

  `/radmin user-handle` to remove an admin

  `/vote user-handle`  to vote to add user to ban list and expel


