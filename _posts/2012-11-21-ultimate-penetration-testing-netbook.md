---
layout: post
title: Ultimate Penetration Testing Netbook
date: '2012-11-21T22:37:00.001-07:00'
author: JM
tags: 
modified_time: '2012-11-24T03:49:05.205-07:00'
thumbnail: http://2.bp.blogspot.com/-9vf96S5WHqg/UK2xro42-AI/AAAAAAAAAKQ/DEenxK5p-M8/s72-c/Screen+Shot+2012-11-22+at+3.00.43+PM.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-8027667875299510669
blogger_orig_url: http://www.pwnage.io/2012/11/ultimate-penetration-testing-netbook.html
---

## Intro

With all of this free time I've been given after finishing all remaining aspects of my degree I've taken on some projects to keep my mind fresh and busy prior to stepping into the working world. Through some interesting events I've acquired a netbook that was given to my girlfriend after some unfortunate circumstances. I actually used some of my IT skills in this circumstance to help her out, and a very kind person gave this to us. After this netbook was no longer needed I decided to convert this into the ultimate penetration testing device.

Tasks I've come up with so far:

- Installing [backtrack](http://backtrack-linux.org/) on the device via USB since it does not have a CD/DVD drive
- Locking it down with full disk encryption
- Mounting my already acquired wireless device with some velcro: ALFA Network AWUS036H
- Loading all of my established tools
- Anything else I can think of - will update :)

## Installing Backtrack 5 R2 Via USB

Alright, so simple enough really, there's some more in depth (using dd, etc) to accomplish this, but obviously this is the most straight forward approach. First, grab an image from [http://backtrack-linux.org/downloads/](http://backtrack-linux.org/downloads/) (obviously you'll need an iso of whatever you prefer, being KDE/GNOME, 32/64-bit), once that is done, go over and grab the handy utility "unetbootin" which will run on whatever you are currently running: [http://unetbootin.sourceforge.net/](http://unetbootin.sourceforge.net/). 

Next you'll want to plugin your trusty USB (unfortunately Backtrack is pretty huge because of all the toolsets, you'll have to use a USB that is at least 8GB). Once you've done that, open up unetbootin and you'll see this:

[![](http://2.bp.blogspot.com/-9vf96S5WHqg/UK2xro42-AI/AAAAAAAAAKQ/DEenxK5p-M8/s400/Screen+Shot+2012-11-22+at+3.00.43+PM.png)](http://2.bp.blogspot.com/-9vf96S5WHqg/UK2xro42-AI/AAAAAAAAAKQ/DEenxK5p-M8/s1600/Screen+Shot+2012-11-22+at+3.00.43+PM.png)

Now, you could have used their available distribution selection and it would have done everything for you, but I found that it was just easier to download the ISO initially. Next, you'll want to select Diskimage and leave it as an iso. Click ... and you'll be presented to navigate to the location of the ISO (being the backtrack ISO that was just downloaded). Then select your USB in the Drive drop down make sure it's the right one! Then click OK and unetbootin will take care of the rest.

Okay, so now you've got a nice little USB, let's boot into Backtrack 5. First things first, you'll have to set either your BIOS settings to boot from USB, or you'll have to select it from the Boot Menu on startup. This will differentiate based on your laptop or netbook, however, this is also very straight forward.

Once you've done that, select your appropriate environment that Backtrack will boot into (it's fine to select the default). This will bring you to the old:

> root@bt: ~#

Then you'll want to type in :

> startx

Which will launch into the GUI multi-user environment. I'm going pass the next step off to this [fantastic tutorial](http://www.infosecramblings.com/backtrack/backtrack-5-bootable-usb-thumb-drive-with-full-disk-encryption/) that will walk through the encrypted backtrack installation process. Keep in mind of course that this is in fact for a portable USB device, but since in this case we are working with the laptop's hard drive, but the process is essentially the same, just avoid the sections which make special installation exceptions for the USB device.

## Mounting Wireless Device
Once this entire process is complete then the next steps are fairly straight forward. I wanted to mount my handy ALFA Network wireless card for all of my wireless needs, all I needed next was some velcro to stick the sucker on the back.

To be continued...
