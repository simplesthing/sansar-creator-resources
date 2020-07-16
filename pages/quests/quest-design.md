---
title:  "Quest Design" 
summary: "This guide has been written for creators using the Sansar Quest System to provide some guidelines and inspiration, regardless of their past experience."
category: quests
tags: [ games ]
sidebar: sidebar
permalink: quest-design.html
topnav: topnav
last_updated: October 23, 2019
---

##  What is a Quest?

A quest is a bite-sized activity for a player to do while in Sansar. Quests allow you, as a creator to present story and characters, to provide specific tasks to players, to guide them through your experience, and then reward them for completing the tasks. Quests provide structure and help you to design a compelling player path, giving players fun things to do while they are experiencing your creation.

In Sansar, quests are delivered to players via several different UI panels which you configure with text and state changes to indicate what you'd like them to do.

## Quest States

When designing a quest, it makes the most sense to think about it in terms of states, in the script API they are represented by the [QuestStateEnum](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/QuestState.html).

 - **None** (Not Available):  The quest has not yet been offered to the user. This is a quest that exists, but isn’t available to a player for any number of reasons. You may choose to gate some quests behind specific milestones in your experience, such as completing certain previous quests or other tasks that you provide as part of your World.

 - **Offered** (Available, but Not Acquired): The user has been offered the quest. A quest can be available to a player, but not yet acquired or accepted by the player. Quests can be thought of as opt-in activities, in that they require the user to decide to take them on. You also have the option as a creator, to push a quest offer to users automatically when they enter your World or meet requirements that you determine.

 - **Active** (Active / In Progress) : A quest is active when a player has accepted the quest, and is currently in the process of completing the objectives. When a quest is active, the information is available to players in their Quest Log.

- **ObjectivesCompleted** : When the player has completed the objectives of your quest, you have the option to have that quest complete automatically, or have them enter into the `SelectReward` state.

- **SelectReward** : The user has completed all the objectives for this quest, and turned it in, but still has to select rewards.

- **Completed** : The user has completed the quest and it will be removed from their quest log.

## Objective States

Objectives are represented by the [ObjectiveStateEnum](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/ObjectiveState.html). 

*Quest Creator Objective Initial State* <br>
![Objective Initial States]({{ site.baseurl }}/images/quest-design/objective-initial-states.png)

 - **None**: The objective state is not set, probably the user is not on the quest. 

 - **Active**: The objective is currently active and available to complete for this user. An objective is active when a player has accepted a quest and has fulfilled prerequisites to completing that objective.

- **Hidden**: The objective is hidden from the user. This can be used to only show clues when the time is right, adding an element of discovery.

- **Locked**: The objective is locked for this user and can not be completed. An objective is locked when prerequisites for completing that objective have not been fulfilled. This can be a way to enforce having objectives completed in a specific order.

- **Completed**: The objective has been completed for this user.


## Quest Character

The quest system includes the concept of a character. A character can be thought of as an entity that can give quests and complete quests. Any object can become a character by attaching the proper scripts to it while editing your World. Many characters are visually human or human-like, but animals, computer terminals, or even a rock could become a character and offer quests, if you want it to.

There are technically two kinds of Quest Characters in Sansar, although only one of them is available to our creators at the moment.

### Single Quest Giver Character

The Single Quest Giver is what is available to creators, it offers a single quest via [interaction, trigger or controller]({{ site.baseurl }}/quest/2019/10/23/quest-simple-script-reference.html). 

### Storyline Quest Giver Character

The Storyline Quest Giver Character is used to offer multiple quests along a storyline. Agent Primus and Agent Forma are examples of Storyline Quest Giver Characters in Sansar today.

***At this time the Character Quest Giver is only available for Sansar Studios quests, but is something we'd like to do down the road**

## Guidelines for Quest Text Fields

There are several fields for each quest and each has specific considerations to keep in mind when writing. This has been written to inform you, as a creator, what our intentions were when we were designing the system. Consider this a baseline, giving you a place to start when you’re creating your first quests.

### Quest Information

![Quest Info]({{ site.baseurl }}/images/quest-design/quest-info.png)


#### Quest Name/Title *(labeled name in creator tool, called Title in [script API](https://lindenlab.github.io/sansar-script.github.io/assets/Documentation/Sansar.Simulation/QuestDefinition.html#P:Sansar.Simulation.QuestDefinition.Title))*

Each quest should have a unique name that in some way references the task at hand. Names can be general, specific, humorous, or serious. They can be a single word, or a short phrase, although names always need to be fewer than 100 characters. In most cases, there is no need to include the word “quest” as part of the name, although there are some circumstances in which it may make sense to do so.

#### Description 

Where the main story bits go. This field is always written from the point of view of the character that is offering the quest, in the voice of that character. Usually the character will introduce the main goal of the quest, along with some flavor text. Essentially, you should focus on the main point of the quest here, i.e. what you’d like the player to do. However, there usually isn’t a need to give exact, specific instructions, as you’ll have the objective text to do that. Feel free to have your character be a bit mysterious, vague, confused, or unsure in the story itself, because you can then clarify what the player will be doing in the objective text. This field has a 350 character limit

#### Summary

The summary (also called Log) relates the main goal of the quest in a straightforward way. It appears in the thumbnail view for the quest, in the main Quest panel. This is never written in character voice. It should be a single phrase or sentence that conveys the goal. For this field, we do not use punctuation at the end of the phrase or sentence. The character limit is 100, and we keep the summary to one line whenever possible.


#### Completion Message
This text is seen when a player completes the objectives of the quest and a completion modal is displayed. This text is written from the point of view of the quest narrator, and should generally convey a message of satisfactory completion: *“*Thanks for doing that,”* or *“Nice work - please take this reward.”* 

### Objective Information

![Quest Info]({{ site.baseurl }}/images/quest-design/alpha-objective.png)

#### Objective Name

Objective titles are to-the-point instructions that explain what the player needs to do to complete a particular objective. They should be short. URI's to other in world scenes will not parse into clickable links in this field as they are intended to go into the description text.

#### Objective Description

The description is written in the same tone, but will go into more detail and often explain, step by step, what the player should do. If the objective tells the player to travel to another world, consider using an active Sansar URI that the player can click to travel there immediately. Objective descriptions support the use of links to any worlds within Sansar, displaying the name of the world automatically. Objective titles do not support links.

The objective title is limited to 100 characters or less, and the objective description is 250 characters or less.


## Quest Design Tips
The following is a collection of best practices, do’s & don’t’s and tips to help you when you’re creating your own quests in Sansar.

### Guide the Player Experience
When you’re thinking about what quests you’d like to add to your experience, think about the overall design and structure of your World, and consider how the player will both move through the space you’ve created while completing the tasks you’ve set forth. The path the player follows should feel logical, opening up more and more of your experience over time. 

You may want to be very careful with highly repetitive tasks, or requiring the player to return to one specific location over and over. Players can easily become bored with such activities and leave your experience without completing it.

Keep them engaged by providing different types of tasks, and by revealing a little of the story with each step. Like a good novel, your quests should keep the player wondering what might come next.

### Keep it Short
As discussed above, quest text fields have specific limits. These limits were put into place to help you communicate better with players engaging with your content. Limited space means that you need to be choosy with what you decide to write, and it will help players encountering your quests understand what they are being asked to do, because they don’t have to search for the task in a long block of text. Most players are there to play, not to read -- so keep it short and focus on the gameplay.

### The Player is the Hero
When you’re designing your quests, your thoughts should always be on the experience you are providing to your player. What do you want them to feel? How should they experience a specific event? A good tactic to take, in general, is to focus on making the player feel like they are awesome, helpful or essential to the events that are unfolding. They need to feel central - the hero in your story - like their actions matter within the context of your World.


Even if you’re creating a story in which the player is a supervillain, or an experience that is intended to evoke sadness, you can still make sure that the player feels like an integral part of that experience. Make their part meaningful and players will enjoy it.

### Be Clear
Quests should always make clear what is being asked of the player. After reading your quest, if the player is saying to themselves, “What am I supposed to do here to complete this?” then you haven’t done a great job as a designer. Best practice is to always name the goal, the location where it can be completed, and explain what to do once the player has finished the task.

Being clear does not mean you can’t have any mystery in your quests. It’s very possible to be clear about the actions a player should take without revealing every detail. You can tell the player they need to find a sacred gem within the valley of shadows without telling them exactly where it might be, for example. Be clear about the player’s goals, and then leave some of the finer details for them to discover on their own.

A great way to manage this is to tell your story in the description text, and then use objective text to clarify the steps you’d like the player to take to complete the quest. When you test the quest, you can determine where more details might be needed to help players along.

### Craft a Compelling Story
When creating an experience, spend time determining your story arc, and how you’d like to deliver that to the player. Any time you ask the player to complete a quest, you should also give them a compelling reason as to why they are being asked to do that particular activity. The “why” of your quest should be to tell an essential part of your overall story. You can create quests that ask players to do things without story attached, but it will feel much less meaningful to your players.

For example, I could easily create a quest that says:

“Climb to the top of that mountain over there and jump 17 times in a row.”

Or I could create a quest that says:

“Pssst! You there! I am Professor Sniggles, and I have just completed my analysis of planetary alignment. In a mere 3 minutes from now, the timing will be right to push our planet - yes, this world we exist in now - into a parallel universe! You must help me. Please - go to the top of that mountain just across from here, and START JUMPING! Hurry!”

Which one would you remember? Which one would you rather do? Craft a compelling story and you’ll hold player interest.

### Keep it “Bite-Sized”
Using the Sansar quest system, it is possible to create a quest with an enormous number of objectives. However, delivering all of those objectives in a single quest has a very high chance of feeling overwhelming to your player. When people feel overwhelmed, they will give up on your quest and do something else. So you should always strive to break up your tasks in a way that feels doable to your player.

We’ve found that in general, good quests have 8 or less objectives, with a large majority of our internal quests having around 4 to 5.

## “Going Live” with your Content
When you publish your World, your quests and content will be live for other players! Before you click that “Publish” button, here are some things you might want to think about.

### Testing is a Part of Content Creation Too
It’s fun to come up with an idea and begin implementing it into your World. You’ll need to write a story, script your events, and place objects. Watching it all come together is the best. But before you put your content live, you need to test it. A lot. Part of creating quests is testing to make sure that it plays exactly as you intended. 

So once you’ve gotten everything in, play through your quest, multiple times. If you have any complex scripting, play through with a critical eye, considering every point where it might break. As the designer, you know it’s supposed to be played, but that doesn’t mean others will do things exactly the way you want. Try things that are unconventional and see what happens. Players are sometimes unpredictable - what happens if someone does something in the wrong order? Or follows a different path to the goal? What if they stop in the middle of your quest, log out and come back later? How will you handle those possibilities? 

As a content creator, this is your job; to put yourself in the place of the player and ensure that the possible experiences they might have are what you are aiming for. If you find problems, come up with solutions, fix them, and then test again.

When you’ve tested your quest(s) extensively, fixed any bugs and typos, and added your rewards, your quest is ready for other players. If you’re feeling confident that you’re done, that’s when you know it’s time to make your quest available to others.


### Once it’s live, it’s live
Publishing your content should be thought of as a one way street. You turn it on, it’s live, and you leave it alone. Generally you shouldn’t be editing a live quest, ever. If you think you aren’t quite done and you want to make changes, then wait. Don’t push your content live until you’re sure it’s ready.

### Oops, I Need to Fix Something
There are times where you may find yourself wanting to make a change to live content for a number of reasons. Maybe you missed a typo and want to correct it, maybe a player found a serious bug and you need to address it with a fix, or maybe you accidentally added the wrong reward to your quest and want to change it. (All of these reasons point to more testing, next time around.) But there are times where you just need to fix something. So what should you do?

First, let’s go over the things that can happen if you change live content.

If you go in to make a fix, you may end up causing another bug. This is the biggest reason that you shouldn’t touch live content. You never know what might end up breaking when you make a change.
Note: This is the game industry standard. Live content is locked and untouchable in most cases.
By changing a quest, you will create a situation in which some players have the old version of your quest, and some players have the new version. A quest will not automatically update to the latest version once a player has accepted it. So you cannot assume that fixing a bug will fix the problem for players that already have your quest, because they already have the version with the problem. Only players that accept the quest after the fix will see the change. There are some nuances here, but this is generally the case.
If you’ve added a reward to your quest and then decide to change it, it will not change for all players. Only players that accept the quest after you make the change will get the new reward.

### What Should I Do?
If you need to make a change to a quest, you should immediately unpublish your World so that other users can’t get stuck or see the problem. Once your World is unpublished, you should choose which course of outcome you’ll take: 

Make a change to the live quest, or
Make the broken quest inactive (remove it from your World and scripts), and replace it with a brand new duplicate of the original with fixes in place, or
Delete the broken quest altogether, and replace it with a brand new duplicate of the original with fixes in place.

There are rare times when you might determine you’re OK with changing your live quest - such as fixing a typo. Some players might have the typo in the quest text, but it’s not preventing them from completing the quest. You fix the typo quickly and move on. But there are other times where you may need to make a major change. In those cases, you should choose to replace the quest with a new one.

If you make your original quest inactive, it may remain in player logs, even if no new players can now accept it. Then if you publish your World again later, a player with the broken quest may attempt to return to your World and complete it. It’s still a problem. 

However, you can delete the broken quest entirely. When you delete a quest you’ve created, it will immediately remove it from all players that had it. It is gone, forever. Once you’ve deleted the problem quest, you can create a brand new one, and make sure it contains your fixes. With a new quest available, all players will now be able to accept and play through the fixed version. 

Of course, replacing your quest does require some extra work on your part. You’ll need to make sure that you change any scripting reflect the new quest, and you’ll want to test, test, test before putting it live again!

### What to do if you find a bug... 

As always use Sansar Script Library Debugger scripts with the console (ctrl-d) as a first step to inspect your quest and objective states. If you think you have found a bug in the Quest System its always helpful to post a question in the #quest channel of our discord server as a first and possibly fastest step toward a resolution. If you do not get an answer from discord then [file a ticket on our support forum](https://help.sansar.com/hc/en-us/requests/new).

## Quest Writing Best Practices

Referring to Players in Quest Text
When referring to a player within quest text, the common universal moniker is “Sansarian.” A Sansarian is any player that visits Sansar. However, players can also be greeted by name by using the tag {Player.Name} within the quest description, progress, and completion text. It is fine to have a character call a player by name where it makes sense, although be aware that it is easy to overdo it, so use sparingly. One usage per quest field is usually the guideline. Note that the tag is case sensitive and must be typed exactly as the examples show.

*“Greetings, {Player.Name}, I am quite pleased that you have completed the task.”*

*“{Player.Name}, this doesn’t seem right. What should we do?”*

Outside of these names, there may be others that work fine, depending on the character speaking them, such as “Pal,” “Friend,” or even “Hotshot.” Keep in mind that we have no way of identifying the gender of a character, so any names should be universal, working for everyone. Names like “Bro,” “Dude,” or “Sister,” wouldn’t work for all players and are best avoided.

### General Writing Style Guidelines
These are some basic rules and guidelines that we use here at Linden Lab when we’re writing text for Sansar. Feel free to use these tips when creating your own quests!
Quotation marks
These can be used liberally for irony, to have one character quote another character, to reference buttons in the UI, but also can be used to set a phrase off from the rest of the description to make it sound like a TV or radio spot you are listening to. 

“The Socialite Guild: it’s the place to be!”

“Click “Visit” to travel to a World.”

#### Numerals
Typically quest objectives use Arabic numerals for numbers like, “Throw away 8 pieces of trash” because that number needs to be dynamic and code-based. In the quest description text, best practice is to spell the number out such as, “Step on three different Avatar Scan points.” 

#### Single Space after Periods
Do NOT put two spaces after a period. Ever. Double spacing is a remnant of typewriter conventions using pre 1970’s monospaced fonts.

#### Dashes, Ellipses, & Colons
Feel free to bend the rules for quest description or completion text to get the right impact because:

 *“Sometimes - a dash creates the perfect cadence!”*

 *“It’s confirmed: a colon can make it feel more definite.”*

*“Sometimes…an ellipses… is the perfect way to…how you say… build to a final reveal…?*

It’s all about making it more…fun…to…read - and more expressive, as if the character is actually speaking. Take liberties in quest text with correct spelling, grammar, or punctuation if it makes it read better. 

There are certain situations in which it’s better to be straightforward, such as for quest objectives which we do not write in character voice and should always use correct spelling and grammar.

#### To Comma, or Not to Comma
This depends on personal preferences, pace you want to convey, and how legible or illegible this makes a string of adjectives or adverbs. 

Use a comma to separate coordinate adjectives. You could think of this as "That tall, distinguished, good looking fellow" rule (as opposed to "the little old lady"). 
Use a comma to separate the elements in a series (three or more things), including the last two. "He hit the ball, dropped the bat, and ran to first base." In most cases, the comma before the "and" is unnecessary, which is fine if you're in control of things. However, there are situations in which, if you don't use this comma (especially when the list is complex or lengthy), these last two items in the list will try to glom together (such as phrases that contain an additional “and” like “macaroni and cheese”).

#### Using All Caps
Use all caps as desired to designate yelling or to emphasize the way a character speaks. For example, Agent Forma commonly communicates in this fashion:
 	
*“I. CAN'T. EVEN. The Myriad seems to be all over the place here! Do you think you could follow the trails and ... see how bad it is? You can start by checking out the portal behind me. It doesn't look right AT ALL.”*

Characters should never speak completely in all caps. It should be used to designate emphasis on specific words or phrases.

#### Italics, Bolding, and Underlining
Currently quest text fields do not support these styles. If this changes in the future, this section will need to be written to determine when these styles are appropriate in quest text.

#### Copyright, Trademark, registration symbols
There usually is no need to use the following symbols in quest text: ™ ©  ®

These symbols come into play if you have legally obtained the trademark or copyright for a product and want that indicated in game. Do not use these symbols in jest or to create a fictitious brand within the game as there is a legal liability.

#### Pop Culture References and other Web/Game Slang
Slang is OK if it underlines the specific nature of a character, but is usually the exception and should be used sparingly. It can be a good inside joke for gamers, but excessive use may impact immersion and believability. Consider that the quest may be active for players for years, and that pop culture references usually do not age well over time. This needs to be handled on a case by case basis, as it may work great for some experiences and not for others.

#### Web Links
Links to worlds in Sansar, or even to the Sansar website or shop are allowed anywhere that it makes sense to do so.

Don’t insert links to external web pages like www.google.com, even if they are not hot links in any text fields. It’s a legal liability. Don’t make fictional web pages up like www.thisisafakewebpage.com because it may at some point turn into a real web page and become a legal liability. 

#### Brand Names
Make up your own brands, characters and elements if needed, unless the quest is being developed to support an established partner. Using a real brand, celebrity name or company name is a legal liability. This includes phrases like “Grab a Kleenex” and “Go Xerox it” and “Just Google it sometime.” 

#### Foreign Accents
Expressing foreign accents through text could easily be considered borderline incorrect; however it does happen in many other games, and it may make sense in certain circumstances to create text and dialogs with an “accent” for foreign characters. These characters may not even be from a place in the real world, and so therefore it is easier to create an accent without being offensive to a particular race or nationality. 

Of course, we always have the option of writing text out normally, and then having a voice actor employ a specific accent when recording dialogue. The character Tali-Zorah in the Mass Effect series is an example of a character with a strong accent, but this is never indicated in any way through the text in the game - it’s all voice work. 


## Sensitive Subject Matter in Sansar
There are so many potential topics and experiences that could be created within Sansar. When you’re thinking about your own design, there are several topics that we should be careful with in regards to quests and worlds within Sansar. Keep in mind that we strive to keep all of our content consistent with what you would find in games that have a T (TEEN) ESRB rating.

The following topics should be considered sensitive subject matter best avoided with exceptions in the case of the creation of historical content intended to educate.

- Politics
- Religions & holidays
- Sex
- Bathroom Humor
- Violence / Death
- Substances
- Expletives, Profanity and Hate Speech
- Cruelty to Animals
