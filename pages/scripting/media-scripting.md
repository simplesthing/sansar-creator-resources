---
title:  "Media Scripting"
summary: "Scripting for the media surface"
category: scripting
sidebar: sidebar
tags: [media_surface ]
permalink: media-scripting.html
author: Binah
last_updated: August 15, 2019
---

## How to control the media source

Media scripting in this post is referring to controlling a [media surface]({{ site.baseurl }}/media/2019/08/12/media-surfaces.html) through scripts. 

Like sounds, the media source for a scene can be controlled globally or on a per-user basis.  Scenes only
support a single media source though, and there is no functionality to save off a screenshot or otherwise
freeze a media source to an object.  When the media source updates, all surfaces that use the media source
material will update to reflect the new data.

The two main interfaces for accessing the media source are both in the [Sansar.Simulation.Namespace]({{ site.baseurl }}/assets/Documentation/Sansar.Simulation/index.html). 

To change media for the scene, everyone in the sees the same thing, you would use the [ScenePrivate.OverrideMediaSource]({{ site.baseurl }}/assets/Documentation/Sansar.Simulation/ScenePrivate.html#M:Sansar.Simulation.ScenePrivate.OverrideMediaSource(System.String,System.Int32,System.Int32)) method.

     // scene-wide, all users will see this
    ScenePrivate.OverrideMediaSource("https://www.sansar.com/"); 

To change media for an agent, per user, you would use the [AgentPrivate.OverrideMediaSource]({{ site.baseurl }}/assets/Documentation/Sansar.Simulation/AgentPrivate.html#M:Sansar.Simulation.AgentPrivate.OverrideMediaSource(System.String,System.Int32,System.Int32,System.Action{Sansar.Script.OperationCompleteEvent}))
    
    // only for this agent
    agent.OverrideMediaSource("https://atlas.sansar.com/experiences/sansar-studios/");  

Here is a sample script to change the media source for a specific user when they interact with the object:


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


## MediaCommand Script

Leslie Linden has an all in one media controller script, [MediaChatCommand.cs](https://github.com/lindenlab/sansar-script/blob/master/Users/leslie-linden/commands/MediaChatCommand.cs) that will accept chat shorthand commands to override the media source and format the urls.  Let's take a look at it.

    using Sansar.Script;
    using Sansar.Simulation;
    using System;
    using System.Collections.Generic;

It extends  [SceneObjectScript]({{ site.baseurl }}/assets/Documentation/Sansar.Simulation/SceneObjectScript.html), the primary script type in Sansar, can be used on content that is in the scene.

    public class MediaChatCommand : SceneObjectScript
    {

Public parameters for restricting access to scene owner only or allowing all users in the scene to control the media through chat commands. Public properties can be modified in the editor in Sansar.

        [Tooltip("All users can type media chat commands if this is on")]
        [DisplayName("Restricted Access")]
        [DefaultValue(true)]
        public bool RestrictedAccess;

Alternately a list of users that have access to control the media through chat commands.

        [Tooltip("Comma separated list of user handles for those granted restricted access")]
        [DisplayName("Restricted Access User Handles")]
        public string AllowedAgents;

Private objects to store allowed agents and the list of command strings.

        private HashSet<string> _allowedAgents;
        private Dictionary<string, string> _commandsUsage;

Initialize the script and private objects, populate the command dictionary. 

    public override void Init()
    {
        _allowedAgents = SplitStringIntoHashSet(AllowedAgents);

        _commandsUsage = new Dictionary<string, string>
        {
            { "/help", "" },
            { "/media", "[url]" },
            { "/yt", "[video-id] (optional start time)" },
            { "/youtube", "[video-id] (optional start time)" },
            { "/ytlive", "[channel-name]" },
            { "/ytpl", "[playlist-id]" },
            { "/twitch", "[channel-name]" },
            { "/twitchv", "[video-id]" },
            { "/vimeo", "[video-id]" },
        };

Subscribe to Chat and add `OnChat` callback to listen to commands.

        ScenePrivate.Chat.Subscribe(Chat.DefaultChannel, OnChat);
    }

A utility function used to separate allowed agents arguments passed in from public members.

    HashSet<string> SplitStringIntoHashSet(string commaSeparatedString)
    {
        HashSet<string> hash = new HashSet<string>();

        string[] separateStrings = commaSeparatedString.Split(',');
        for (int i = 0; i < separateStrings.Length; ++i)
        {
            string separateString = separateStrings[i].Trim();
            if (!string.IsNullOrWhiteSpace(separateString))
            {
                hash.Add(separateString);
            }
        }

        return hash;
    }
    
Another utility function to determine who has access.

    bool IsAccessAllowed(AgentPrivate agent)
    {
        if (RestrictedAccess)
        {
            bool accessAllowed = false;

            if (agent != null)
            {
                // Always allow the creator of the scene
                accessAllowed |= (agent.AgentInfo.AvatarUuid == ScenePrivate.SceneInfo.AvatarUuid);

                // Authenticate other allowed agents
                accessAllowed |= _allowedAgents.Contains(agent.AgentInfo.Handle);
            }

            return accessAllowed;
        }

        return true;
    }

The real meat of the script is in the `onChat` callback. 

    void OnChat(ChatData data)
    {

Get the agent that typed into chat, and check if they are allowed to use the commands.

        AgentPrivate agent = ScenePrivate.FindAgent(data.SourceId);
        if (!IsAccessAllowed(agent))
            return;

If allowed parse the string entered into chat separated by spaces.

        string[] chatWords = data.Message.Split(' ');

        if (chatWords.Length < 1)
            return;

Pull of the the first string of characters and check the `_commandUsage` Dictionary to see if there is a match.

        string command = chatWords[0];

        if (!_commandsUsage.ContainsKey(command))
            return;

If so  find the case that matches the commands.

If the help command was used, list the values in the `_commandUsage` Dictionary back into the private chat.

        if (command == "/help" || chatWords.Length < 2)
        {
            string helpMessage = "MediaChatCommand usage:";
            foreach (var kvp in _commandsUsage)
            {
                helpMessage += "\n" + kvp.Key + " " + kvp.Value;
            }

            try
            {
                agent.SendChat(helpMessage);
            }
            catch
            {
                // Agent left
            }

            return;
        }

If its not a help command then it must be a media command therefore we need to initialize the mediaUrl string variable.

        string medialUrl = "";

Parse the  variations out into their proper url formats and assign to the `mediaUrl` variable.

        if (command == "/media" || chatWords[1].StartsWith("http"))
        {
            medialUrl = chatWords[1];
        }
        else if (command == "/yt" || command == "/youtube")
        {
            string videoId = chatWords[1];
            medialUrl = string.Format("https://www.youtube.com/embed/{0}?autoplay=1&loop=1&playlist={0}&controls=0", videoId);

            if (chatWords.Length > 2)
            {
                int startTime = 0;
                if (int.TryParse(chatWords[2], out startTime))
                {
                    medialUrl += "&start=" + startTime;
                }
            }
        }
        else if (command == "/ytlive")
        {
            string livestreamId = chatWords[1];
            medialUrl = string.Format("https://www.youtube.com/embed/live_stream?channel={0}&autoplay=1", livestreamId);
        }
        else if (command == "/ytpl")
        {
            string playlistId = chatWords[1];
            medialUrl = string.Format("https://www.youtube.com/embed?%20listType=playlist%20&list={0}&loop=1&autoplay=1&controls=0", playlistId);
        }
        else if (command == "/twitch")
        {
            string channelName = chatWords[1];
            medialUrl = string.Format("http://player.twitch.tv/?channel={0}&quality=source&volume=1", channelName);
        }
        else if (command == "/twitchv")
        {
            string videoId = chatWords[1];
            medialUrl = string.Format("http://player.twitch.tv/?video={0}&quality=source&volume=1", videoId);
        }
        else if (command == "/vimeo")
        {
            string videoId = chatWords[1];
            medialUrl = string.Format("https://player.vimeo.com/video/{0}?autoplay=1&loop=1&autopause=0", videoId);
        }

Get ready to send the command, but first setup a boolean flag to catch throttling exceptions, to be used in debug log message if a throttle exception occurs as well as to retry the command.

        bool throttled = false;

        try
        {

Override the media source.

            ScenePrivate.OverrideMediaSource(medialUrl);

Send messages back to chat and debug log of event success.

            agent.SendChat("Media URL successfully updated to " + medialUrl);
            Log.Write("New media URL: " + medialUrl);
        }

Flip the throttle boolean and log the throttle exception.

        catch (ThrottleException)
        {
            throttled = true;
            Log.Write("Throttled: Unable to update media URL.");
        }
        catch
        {
            // Agent left
        }

try again if the command was throttled.

        if (throttled)
        {
            try
            {
                agent.SendChat("Media URL update was throttled.  Try again.");
            }
            catch
            {
                // Agent left
            }
        }
    }
}