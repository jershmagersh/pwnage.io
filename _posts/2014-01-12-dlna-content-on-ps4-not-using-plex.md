---
layout: post
title: DLNA Content On PS4 Not Using Plex
date: '2014-01-12T21:02:00.001-07:00'
author: JM
tags: 
modified_time: '2014-01-12T21:09:14.812-07:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-2707978049967521468
blogger_orig_url: http://www.pwnage.io/2014/01/dlna-content-on-ps4-not-using-plex.html
---

## Intro

This isn't so much of a security post as a media content/streaming one. I was lucky enough to receive a Playstation 4 for Christmas this year and noticed it was lacking in the department of DLNA content. Essentially there's no way to stream local content at this point in time to your Playstation on your home network via their tools.

I'm not sure what the motive of this would have been? I'm leaning mostly toward an anti-piracy effort or an effort to increase the use of their streaming services. The only tutorials I've seen online thus far involve using a Plex account to stream content, but there's really no need if the Playstation's browser provides flash support.

## JWPlayer & Server Setup

I ended up using a free (for personal use) open source project called JWPlayer. Head over to [their signup page](http://www.jwplayer.com/sign-up/) and [download](https://account.jwplayer.com/static/download/jwplayer-6.7.zip) their sourcecode in a zip file (preferably to the Web Server that you'll be hosting it from). Next, you'll need to setup a web server somewhere on your network to host the provided scripts. I did the following with an old P4 machine running [lubuntu](http://lubuntu.net/):

`sudo apt-get install apache2`

This will install Apache Web Server on the local machine and start the service automatically.

Next you'll want to copy that zip file to `/var/www` where your web content will be hosted and unzip it:

`sudo cp jwplayer-6.7.zip /var/www`

`cd /var/www/`

`sudo unzip jwplayer-6.7.zip`

Open up a browser and go to `http://localhost/jwplayer/README.html`

This will provide instructions on some basic setup of the javascript libraries. I ended up with:

`cat play.html`

```
<html>

<script type="text/javascript" src="jwplayer/jwplayer.js"></script>

<div id="myElement">Loading the player...</div>

<script type="text/javascript">

    jwplayer("myElement").setup({

        file: "/uploads/video.mp4",

    });

</script>

</html>
```

Nothing fancy, essentially the provided embedded code in an html file with adjustments to the javascript paths. The default player dimensions are small when you hit the initial page, but you can full screen it from the playstation browser which will fit the dimensions of your screen.

You'll want to upload your content to the same server under /var/www/uploads and link HTML files to with their names in the jwplayer function call as you can see in the provided code.

Once this is done open up the browser on your PS4 and navigate to your webserver's local IP address with whatever you called your HTML file, for example:

`http://192.168.0.123/play.html`

You'll then see the small jwplayer, click on the play button, then you'll see a full screen button in the bottom right hand corner. Click on this and enjoy the show :)

## Limitations & Comments

Unfortunately only `.mov` and `.mp4` formats are currently supported. I'd suggest grabbing [handbrake](http://handbrake.fr/) to convert any other formats to these.

If you're new to Linux and/or web content in general this might be a little tough for you, but I didn't want to hand hold too much - look at it as being a learning experience. I just wanted to demonstrate that there are other methods besides Plex of streaming content to your Playstation 4.

Thanks for reading,

JM
