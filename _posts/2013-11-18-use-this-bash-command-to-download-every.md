---
layout: post
title: Use This Bash Command to Download Every Phrack Article Ever
date: '2013-11-18T22:29:00.000-07:00'
author: JM
tags: 
modified_time: '2014-03-20T14:38:46.641-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-2497704418641630690
blogger_orig_url: http://www.pwnage.io/2013/11/use-this-bash-command-to-download-every.html
---

Sorry I've been MIA. My working world has been quite hectic recently, and I've been dedicating my research toward it in my off time.

In the mean time, here's a command that will download every Phrack article ever for you:

`i=1; while [ $i -le 68 ]; do curl -o http://www.phrack.org/archives/tgz/phrack$i.tar.gz; i=$[$i+1]; done`

Hopefully I can start posting again soon.

Cheers,

JM
