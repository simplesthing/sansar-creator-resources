---
title:  "Serverless Scene Media"
summary: "Embed web media into their scenes which are rendered by the embedded browser"
category: building
sidebar: sidebar
tags: [github, media_surface]
permalink: serverless-scene-media.html
author: EvoAv
last_updated: August 10, 2019
---

Sansar allows scene creators to embed web media into their scenes which are rendered by the embedded browser. This normally means having to host your content on an external server, especially if you need dynamic content, which would then require managing a database and backend code as well. If you make an update then you need to refresh the page for everyone in the experience. This results in a costly setup, temporary white screens when updating, and unecessary network requests.

But there is a better way.

It is possible to send front-end updates to the page without the use of any API's and let all your visitors see updated content in a very responsive manner using JavaScript. This is done through a lesser known feature of URLs, which is the `fragment`, or the part at the end of the url after the `#` sign. The browser treats that part of the url as front-end navigation, which means it does not do any refresh or network requests, and is usually used by single-page apps that utilize js to update content. This allows you to have static html pages that update dynamically, sync all your visitors with the same view of the page in the experience, and does not require any kind of backend code, meaning all you need to do is host a single html file for your media somewhere on the web.

> **_NOTE_**: The urls in the Sansar embedded browser are limited to 2048 characters, so be careful not to reach that limit.

The down side to this method is it is not persistent, initially, and you are limited to 5 updates per 10 seconds because the sansar api for updating urls (wrongly) assumes it will trigger a network request and therefore throttles it. It is possible to make it persistent by hosting your data on a webservice that has CORS friendly api, and fetch the data with js.

I will now give a quick tutorial on how to setup your own fragment based dynamic web media in sansar, using GitHub as your host.

# Prerequisites

- A github repo and some familiarity with github
- Some familiarity with JS
- A scene with an object that utilizes the media surface shader
- Knowing how to upload scripts to sansar, and basic scene editing.

# Create the page

You will need an html page that reads the fragment of the url on load and every fragment update. Upload the following html page to your repo as `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Serverless Media Demo</title>
</head>
<body>

My dynamic content:
<ul id="content">
</ul>

<script>

</script>

</body>
</html>
```

# Add the JS code

Insert the following js code into the `<script>` tag at the bottom of the html page (above `</body>`). This js will parse the fragment from the url as a json array, by listening to the `hashchange` window event. You will notice I'm using `$()` function in the code, this is not jQuery, but a simple placeholder for `document.querySelector()`, which is defined at the bottom of the code. This is just boilerplate code for framework-less js developement.

This will populate a list element on the page with values in the json array in the fragment. If the json is invalid then the array will be empty.

> **_NOTE_**: You should only use `.innerHTML` if you are emptying an element. Inserting anything else into the element using this method poses a security threat in your app.

```js
(($, $$) => {

  let settings = parseHash(location.hash.substring(1));
  update(settings);

  window.addEventListener("hashchange", () => {
      settings = parseHash(location.hash.substring(1));
      update(settings);
  }, false);

  // Assumes the hash string is made up of only one level json array of strings or numbers
  function parseHash(s) {
    try {
      return JSON.parse(decodeURIComponent(s));
    } catch(e) {
      return [];
    }
  }

  function update(arr) {
    var container = $('#content');
    content.innerHTML = '';
    for (let i = 0; i < arr.length || 0; i++) {
      var elem = document.createElement('li');
      elem.textContent = arr[i];
      content.appendChild(elem);
    }
  }

})((s) => document.querySelector(s), (s) => [].slice.call(document.querySelectorAll(s)));
```

When you are done, upload your changes to the html page in the githup repo.

# Create Github Page

You can now convert your html page to a github page so it will be viewed as a regular webpage by browsers by going to a special url, which you will use to embed in the scene media.

Go to settings:

![Github setttings]({{ site.baseurl }}/images/serverlessSceneMedia/settings.png)

Enable github pages by selecting the branch that will serve as the pages branch, in our case `master`:

![Github setttings]({{ site.baseurl }}/images/serverlessSceneMedia/github-page.png)

The url will appear at the top of that section after the github page is enabled. Now you can test it by going to that url (and appending `/index.html` to the url to reference our page). Just add any json array you like in the fragment of the url, for example `/index.html#[1,"hello world"]`

# Create a Sansar script

For demo purposes, we will create a demo script that will write the names of everyone in the scene into this webpage. Upload the following script to sansar and insert into the floor of your scene. Make sure you have an object in your scene with the media surface shader to see the webpage in world. Don't forget to replace the url in the script with your own that your created above.

```csharp
using Sansar.Simulation;
using System.Collections.Generic;
using Sansar.Utility;

public class VisitorPrinter : SceneObjectScript {
  string Url = "https://darwinrecreant.github.io/serverless-media-demo/index.html";

  public override void Init() {
    ScenePrivate.User.Subscribe(User.AddUser, (UserData data) => {
      Update();
    });
    ScenePrivate.User.Subscribe(User.RemoveUser, (UserData data) => {
      Update();
    });
    Update();
  }

  void Update() {
    List<string> names = new List<string>();
    foreach (AgentPrivate agent in ScenePrivate.GetAgents()) {
      names.Add(agent.AgentInfo.Handle);
    }

    JsonSerializer.Serialize(names.ToArray(), (JsonSerializationData<string[]> data) => {
      if (data.Success) {
        ScenePrivate.OverrideMediaSource(Url + "#" + data.JsonString, 800, 500);
      }
    });
  }
}
```

Now when you enter the scene the page will update with a list of all avatars in the scene and appear on the object with the media surface. It will remain updated as people leave or join the scene. You will notice the updates are instant and sync'ed for all users.

![In-world demo]({{ site.baseurl }}/images/serverlessSceneMedia/demo.jpg)

With this method of using media surfaces, it is possible to stylize your pages with css and of course add much more functionality with js, potentially giving users a way to interact with the media in world, which is in a way better than native interactive media, which is not yet supported, because in this way all users share the same state of the page and see the same thing. With native interactivity each user would have their own session and see different things based on what they do on the page.

At the [Ultimate Disc](https://atlas.sansar.com/experiences/c3rb3rus/test) experience we utilize this trick to have a functioning scoreboard.

![Ultimate-disc]({{ site.baseurl }}/images/serverlessSceneMedia/ultimate-disc.webp)

The demo code is availble here:
[https://github.com/darwinrecreant/serverless-media-demo](https://github.com/darwinrecreant/serverless-media-demo)
