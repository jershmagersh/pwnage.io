---
layout: post
title: Attack of the lol's - How to crash Tamper Data with a POST request tamper
date: '2012-09-06T05:33:00.001-06:00'
author: JM
tags:
- dos
- denial of service
- firefox add-on tamper data
- DOS tamper data
- crash
modified_time: '2012-10-16T21:15:26.097-06:00'
thumbnail: http://1.bp.blogspot.com/-v5HQo3rR5b4/UEiIB3MGwTI/AAAAAAAAAGg/hGrLc0rxUjY/s72-c/Screen+Shot+2012-09-06+at+9.22.35+PM.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-85870379443009298
blogger_orig_url: http://www.pwnage.io/2012/09/attack-of-lols.html
---

So today at work I was working with a number of things looking for some bugs/exploits in web applications. Throughout my day I found a buggy upload form, and as a part of this decided to test the upload form with a large upload request (large file) which naturally would be submitted through a post request. So during my testing I run Backtrack 5 linux, and I have the "Tamper Data" plugin installed in Firefox in order to edit http header requests on the fly. So through my testing I just decided a quick way to generate a file of a fixed size of my choosing would be to do something like this:

> perl -e 'print "lol"x1000000;' > test.txt

This output a file around 30MB due to ASCII characters being 1 byte long and having 1000000 lol's makes 30 000 000 bytes, anyways....

So this file caused me some trouble, actually it crashes "Tamper Data" every time it is submitted. The reason being is that the POST request being sent contains the 10 000 000 lol's in plaint text format and this seems to be too much for it to handle in the POST based tamper screen.

So I wanted to test this on OSX and these are the results:

Firefox goes to 100% CPU (from top in terminal):

[![](http://1.bp.blogspot.com/-v5HQo3rR5b4/UEiIB3MGwTI/AAAAAAAAAGg/hGrLc0rxUjY/s640/Screen+Shot+2012-09-06+at+9.22.35+PM.png)](http://1.bp.blogspot.com/-v5HQo3rR5b4/UEiIB3MGwTI/AAAAAAAAAGg/hGrLc0rxUjY/s1600/Screen+Shot+2012-09-06+at+9.22.35+PM.png)

 Firefox gives a warning that the script is unresponsive:

[![](http://4.bp.blogspot.com/-k6_enveBHQ8/UEiIAJcvc9I/AAAAAAAAAGY/RmFihntw6zc/s320/Screen+Shot+2012-09-06+at+9.21.17+PM.png)](http://4.bp.blogspot.com/-k6_enveBHQ8/UEiIAJcvc9I/AAAAAAAAAGY/RmFihntw6zc/s1600/Screen+Shot+2012-09-06+at+9.21.17+PM.png)

Naturally I hit "Stop Script", and I am presented with this:

[![](http://2.bp.blogspot.com/-O2aNUQ9ju3g/UEiIDioK2yI/AAAAAAAAAGo/Zw1iKFuaf48/s320/Screen+Shot+2012-09-06+at+9.22.56+PM.png)](http://2.bp.blogspot.com/-O2aNUQ9ju3g/UEiIDioK2yI/AAAAAAAAAGo/Zw1iKFuaf48/s1600/Screen+Shot+2012-09-06+at+9.22.56+PM.png)

Tamper Data just stays there even though the buggy script was stopped running! So then I'm like, well alright I'll just close Firefox right? I close all of my open windows, but uhh nope? I can't exit Tamper Data. Not even right clicking from the Dock and clicking on Quit. I'm just stuck with this:

[![](http://1.bp.blogspot.com/-kOyIeCw3oZI/UEiIShTNPgI/AAAAAAAAAGw/xKO32YxwXM4/s640/Screen+Shot+2012-09-06+at+9.25.31+PM.png)](http://1.bp.blogspot.com/-kOyIeCw3oZI/UEiIShTNPgI/AAAAAAAAAGw/xKO32YxwXM4/s1600/Screen+Shot+2012-09-06+at+9.25.31+PM.png)

At this point firefox is at normal CPU again, and I can open windows and it is fully responsive. Interesting though, I'll probably report the bug. Not really a big deal, just for the "lols".

~Josh
