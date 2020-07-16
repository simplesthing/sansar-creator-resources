---
title:  "SHOUTcast Streaming Setup" 
summary: "How to setup a SHOUTcast web audio stream."
category: audio
tags: [ dj, music ]
sidebar: sidebar
permalink: shoutcast-streaming-setup.html
topnav: topnav
author: Binah
last_updated: November 17, 2019
---

Are you an aspiring, or accomplished, musician, DJ, podcaster or radio show host that wants to do your show from inside Sansar? Well good news you can with streaming web audio and this guide will show one way to get that setup.

[SHOUTcast](https://www.shoutcast.com/) is a free cross platform media streaming server software. 

SHOUTcast setup can be as technical or non technical as you can afford to be. If you have ways and means to host your own web server you can host your own free SHOUTCast server, here is a great article that shows the SHOUTcast part - [How To Setup An SHOUTcast Internet Radio Station](https://www.rcast.net/knowledgebase/2/How-To-Setup-An-SHOUTcast-Internet-Radio-Station.html). 

## Direct Stream Links

There are many free solutions to hosting SHOUTcast but all the ones I tried were limited in one special way, they lack providing a [web audio stream URL](https://help.sansar.com/hc/en-us/articles/115004444303-Streaming-web-audio#Getting-web-audio-stream-URLs-from-websites), also called a "Direct Link" that is required to stream in Sansar.

![direct link]({{ site.baseurl }}/images/shoutcast/direct-links.png)

![direct link]({{ site.baseurl }}/images/shoutcast/direct-links2.png)

## Rcast

I tried a few different options in hosting solutions (free-shoutcast, SHOUTcast, Caster.fm) before I chose [Rcast](https://www.rcast.net/) for its price and ease of use. For $3.99 per month you can have you own streaming radio station for up to 100 listeners.

<!-- ![live basic]({{ site.baseurl }}/images/shoutcast/live-basic.png) -->

Once you complete your sign up you will be taken to your Dashboard page. From there choose "View Details" of your Product.

![view details]({{ site.baseurl }}/images/shoutcast/view-details.png)

From there look for a big green button to "Login to Control Panel".

![view details]({{ site.baseurl }}/images/shoutcast/control-panel-login.png)

On the left side of you control panel you will see another green button to start your server.

![rcast control panel start server]({{ site.baseurl }}/images/shoutcast/rcast-control-panel-server.png)

## MIXXX

There are many options for broadcasting your stream with third party broadcaster like, [WinAmp](http://www.winamp.com/) with [Edcast](http://www.shouthost.com/how-to-connect-winamp-edcast-plugin-to-icecast) or [BUTT](https://help.radio.co/en/articles/899769-broadcast-using-this-tool-butt). I went with [MIXXX](https://www.mixxx.org/) for best range of hardware integration options as well as range of use from novice to professional DJ.

I also would like to note here it is preferable that you have a separate computer from the one you are running Sansar on for running broadcaster software as the default settings will be using your computers sound card for input output, the same as Sansar.

[Download MIXXX](https://www.mixxx.org/download/) and install with default settings.

![download mixxx]({{ site.baseurl }}/images/shoutcast/mixxx.png)

Due to licensing MP3 streaming is not supported with MIXXX out of the box, you must download and install the LAME MP3 encoding tool yourself, see the [MIXX Wiki](https://www.mixxx.org/wiki/doku.php/internet_broadcasting#mp3_streaming) for instructions on your platform.

If you're like me you had no idea how to use the tracks you just imported in the installation and all the knobs and buttons are intimidating, no fear watch this quick [Mixxx Tutorial - Beginner's Guide](https://www.youtube.com/watch?v=dm0gFj--2iA) you'll be acquainted with the basics in no time. 

## Connecting to your host

All the information you need to connect to the host can be found on the rcast control panel. In MIXXX open up preferences (ctrl-p on windows), select Live Broadcasting menu item. Your connections should look like below, with your "Login" set to source, encoding set to MP3 and all other values coming from your rcast control panel.

![mixxx]({{ site.baseurl }}/images/shoutcast/mixxx-live-broadcasting.png)

![rcast control panel start server]({{ site.baseurl }}/images/shoutcast/rcast-control-panel.png)

Save your configurations and close preferences. In the top right corner of MIXXX you will see a connection icon, click it to connect.

![mixxx]({{ site.baseurl }}/images/shoutcast/mixxx-enable-live.png)

The icon will turn yellow while connecting, magenta if there is an error or white if the connections was a success. If you are having problems connecting double check your settings and if all else fails read the [MIXXX User Manual](https://www.mixxx.org/manual/1.10/chapters/livebroadcasting.html#mp3-streaming) for more information on connection settings.

![mixxx]({{ site.baseurl }}/images/shoutcast/mixxx-live.png)

## Stream in Sansar

 The Direct stream URL ending in `.mp3` in your rcast control panel is the correct link to use with Sansar.

![direct stream URL]({{ site.baseurl }}/images/shoutcast/direct-stream-url.png)

Follow the Streaming web audio guide on [help.sansar.com](https://help.sansar.com/hc/en-us/articles/115004444303-Streaming-web-audio) for connecting your web audio stream through the Scene Settings panel, or add as is or modify this [Audio Controller Script](https://github.com/lindenlab/sansar-script/blob/master/Users/binah/audio/AudioController.cs) to suite your needs. 


![audio scene settings]({{ site.baseurl }}/images/shoutcast/audio-scene-setting.png)



*Notes*

*[*ShoutVST*](https://www.kvraudio.com/product/shoutvst-by-r-tur) enables streaming sound into Icecast and SHOUTcast directly from VST hosts, ideal for streaming live performances directly from applications like Traktor or Ableton*
