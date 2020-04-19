---
layout: post
title: OSX Application Closer (Kill -9 Everything)
date: '2012-08-19T19:15:00.001-06:00'
author: JM
tags:
- OSX
- terminal
- grep
- ps
- perl
- coding
- command line
modified_time: '2012-08-19T19:16:24.614-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-3740197054624593392
blogger_orig_url: http://www.pwnage.io/2012/08/osx-application-closer-kill-9-everything.html
---

So this isn't really security related but I find that OSX is really slow at closing everything when it needs to shutdown. I guess it does light closing procedures for applications that need saving etc... obviously this is a good thing, but sometimes I just want my stuff to close because I typically save things when they should be saved and don't save things that shouldn't be saved (just took some notes at some point or what have you). So I just wrote this one line for the terminal that will force close all of the currently open applications:

`> sudo ps -Al | grep Applications | perl -e 'while(<STDIN>){chomp; ($uid, $pid) = split(" "); system("kill -9 $pid");}'`

It just gets the list of running processes with ps, greps all of the PIDs that are associated with things running in the /Applications directory and kills them all with a small line of perl! Oh how I looove piping :)

I'm guessing I won't need this when my new shiny SSD comes in the mail today ;)

Anyways, thanks for reading!

~Josh
