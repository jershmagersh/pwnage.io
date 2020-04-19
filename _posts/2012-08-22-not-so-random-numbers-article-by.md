---
layout: post
title: Not So Random Numbers - An Article by Positive Research Center
date: '2012-08-22T17:10:00.000-06:00'
author: JM
tags: 
modified_time: '2012-08-22T17:16:30.370-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-7160043985693505276
blogger_orig_url: http://www.pwnage.io/2012/08/not-so-random-numbers-article-by.html
---

Go check out this article: [http://blog.ptsecurity.com/2012/08/not-so-random-numbers-take-two.html](http://blog.ptsecurity.com/2012/08/not-so-random-numbers-take-two.html) at the Positive Research Center that discusses issues with the generation of PESSID values in PHP.

First it goes over getting the seed value for `PESSID` from `MT_RAND`. A client can synchronize time stamps with the server through what they call "Adversarial Time Synchronization" which consists of sending requests with "dynamic delays" in order to synchronize local microsecond times with the server in question.

As for further information needed are the seed values which make up the `MT_RAND` generation process (which will be used to brute force a `PSSID`). These being the subsequent time measurement variables (time based changes from server microseconds - check out article for more details, simply a difference of `0-3` and `0-4` from the established microsecond time - used by `php_combined_lcg())`, and the server's PID (if running apache server this is given through the apache-status page, woo :)) 

An MD5 phpessid hash can then be brute forced for the seed values, they've provided a nice GUI app for this:

http://bit.ly/RCi5CW

Once you have the seed values then it is easy as:

`(timestamp x pid) XOR (106 x php_combined_lcg())`

In order to avoid this, they suggest using `openssl_random_pseudo_bytes()`, or using /dev/urandom to generate session IDs for password resets instead of the other PHP functions that are vulnerable to this attack. Go check out the article though, it's awesome.
