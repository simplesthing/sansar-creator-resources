---
title:  "YouTube streaming setup"
summary: "How to stream in Sansar using YouTube Live"
category: events
sidebar: sidebar
tags: [ event_hosting, obs, streaming ]
permalink: youtube-streaming-setup.html
author: Binah
last_updated: August 8, 2019
---

Part of the Scripting Office Hours format is to include guest speakers from within the community to share something of note in relation to scripting in Sansar. This document is a guide for one way to stream that content using YouTube Live.

## Open Broadcaster Software 

Download [Open Broadcaster Software (OBS)](https://obsproject.com/), free and open source software for Windows, Mac and Linux.

## YouTube 

Log into your YouTube account and look for a camera icon at the top to the right of the search input box.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/youtube-live.png)

Select the Go Love option

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/go-live.png)

You will need to verify your account 

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/verify.png)

Unfortunately this step takes 24hours to complete.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/24.png)

After you have been verified, go to the Go Live menu option to set up a new stream. Give your stream a name and add permissions, the default is public but you can also stream as unlisted, up to you.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/permissions.png)

You can set the description and topic as you please, recommended Gaming category with Sansar as game name, but whatever you choose is fine.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/gaming.png)

Click 'Create Stream' and in the window that opens up you want to copy the Stream name/key value.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/stream-info.png)

Open up OBS software and click Settings in bottom right of the software.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/obs-settings.png)

In settings go to 'Stream' and select 'YouTube/ YouTube Gaming' and paste your stream key that was copied in previous step into bottom input.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/obs-stream.png)

You might want to check that your video base and output resolutions match, suggested 1920 x 1080 (default).

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/obs-video.png)

You will want to set your Sources to either Window Capture or Display Capture.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/obs-sources.png)

Back in the browser you want to open the YouTube stream settings.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/yt-settings.png)

Set "Ultra low-latency" and Disable DVR if you don't want to record a copy of the stream, if you do then leave the default setting of on.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/yt-stream-settings.png)

Back in OBS controls you want to select "Start Streaming".

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/obs-start.png)

YouTube should auto detect that you have started streaming and will load a preview.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/yt-loading.png)

When the video stream has been connected the Go Live button will turn blue and you are ready to start the stream by clicking that button.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/yt-go-live.png)

Now that you are streaming you need to find the share link by clicking the arrow in the top right of your browser. You will want to copy the video id to give to the Office Hours host to share your stream on the Office Hours media surface.

![YouTube Live]({{ site.baseurl }}/images/streaming-setup/yt-share-link.png)

