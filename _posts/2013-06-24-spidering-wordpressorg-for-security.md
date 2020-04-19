---
layout: post
title: Spidering Wordpress.org for Security Fixes
date: '2013-06-24T09:01:00.003-06:00'
author: JM
tags:
- fixes
- wordpress
- Vulnerability
- Spidering
- exploits
- patches
- patching
modified_time: '2013-07-15T18:55:58.253-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-2445500595491737831
blogger_orig_url: http://www.pwnage.io/2013/06/spidering-wordpressorg-for-security.html
---

## Intro

I first saw this concept in Australia at Ruxcon 2012, which basically comprised of looking at change logs and other available information online to derive vulnerabilities for earlier versions of web applications. This is fairly similar to reverse engineering a patch, say on path tuesday, for information regarding what security fixes that were set into place, but not disclosed publicly. This is obviously much less of a challenge when you have open source projects, which have publicly available change logs of what changes were set into place. I've been picking on WordPress a lot over the past while, especially the plugin structure, but I found this was a nice place to start with spidering of this kind.

## Why?

Well basically people are pretty slow at patching things, especially when it comes to random plugins in their WordPress installation. So if you can find vulnerabilities that were patched privately, and haven't had an exploit released for them (which inherently be exploited by a group of kiddies) then your chances are high that your target hasn't kept up with their patching cycle, and/or you can release an exploit that has had the hard work done for you - if you think finding web app exploits is hard work :).

## Code

I've written some Ruby code that uses the current website structure in place to spider the top 'tagged' plugins for security based information in their change log sections. Basically grabs the tags, sorts them by their font size (font size pertaining to popularity), grabs the page numbers for the plugins (nicely available at the bottom of each initial result page), goes through each of them to find the plugin links, grabs the change log information, and matches it based on security key words.

Grab it on github: [https://github.com/jershmagersh/WPPluginChangeLogScan](https://github.com/jershmagersh/WPPluginChangeLogScan)

## Output

Here's the first few lines found:

```
$ ruby vulnSpider.rb 

Would you like to search for plugins?

y

Getting most popular tags...

Starting with the most popular: widget

Grabbing links...

Plugin: Image Store

URI: http://wordpress.org/plugins/image-store/changelog/

Version: 3.3.0

Log: Security Update

Plugin: Image Store

URI: http://wordpress.org/plugins/image-store/changelog/

Version: 3.2.9

Log: Security Update

Plugin: Feedweb

URI: http://wordpress.org/plugins/feedweb/changelog/

Version: 1.9

Log: Security problem fixed. Redundant code removed.

Plugin: Feedweb

URI: http://wordpress.org/plugins/feedweb/changelog/

Version: 1.7.4

Log: Serious security issue fixed.

Plugin: Feedweb

URI: http://wordpress.org/plugins/feedweb/changelog/

Version: 1.2.8

Log: Security update.

Plugin: Feedweb

URI: http://wordpress.org/plugins/feedweb/changelog/

Version: 1.2.6

Log: Important security update.

Plugin: Easy

URI: http://wordpress.org/plugins/easy/changelog/

Version: 0.8

Log: The security time comes.

All the input fields are now automatically escaped during the widget saving process. All the escapes techniques are defined for each field separately.

If you define your own item (meaning, if you extend the Easy of by your own bricks), doesn't matter if View or Control you can choose from any WordPress built in sanitize, escape function as well as native PHP functions and functions that comes with this plug-in (more in the Documentation).

Plugin: Hit Sniffer Live Blog Analytics

URI: http://wordpress.org/plugins/hit-sniffer-blog-stats/changelog/

Version: 2.5.9

Log: Security Fix: Option to enable hitsniffer dashboard widget for administrators only. ( Thanks to R. Ramos )

Plugin: Hit Sniffer Live Blog Analytics

URI: http://wordpress.org/plugins/hit-sniffer-blog-stats/changelog/

Version: 1.9.6

Log: Security Fix
```
