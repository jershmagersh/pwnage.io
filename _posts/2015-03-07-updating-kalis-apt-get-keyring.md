---
layout: post
title: Updating Kali's apt-get Keyring
date: '2015-03-07T14:33:00.003-07:00'
author: JM
tags: 
modified_time: '2015-03-07T14:33:39.887-07:00'
thumbnail: http://3.bp.blogspot.com/-eU19a8Ryc8U/VPtueoDAkZI/AAAAAAAAAVg/0lQSdeFbbs8/s72-c/pgp.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-3074186199907283202
blogger_orig_url: http://www.pwnage.io/2015/03/updating-kalis-apt-get-keyring.html
---

It appears as though the Kali dev's PGP key has expired, which makes apt-get error out since it cannot verify packages within this state, a fix is to simply update the key:

sudo apt-key adv --recv-keys --keyserver keys.gnupg.net 7D8D0BF6

The identifier at the end was the key in my case (which I'm assuming will be the same in yours), if not you can list your current keys using: apt-key list and you can use those that are distinguished as being expired.

Just thought I'd shoot this forward as I spent some time on this.

![](http://3.bp.blogspot.com/-eU19a8Ryc8U/VPtueoDAkZI/AAAAAAAAAVg/0lQSdeFbbs8/s1600/pgp.png)

[https://xkcd.com/1181](https://xkcd.com/1181/)
