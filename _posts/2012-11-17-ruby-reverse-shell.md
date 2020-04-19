---
layout: post
title: Ruby Reverse Shell
date: '2012-11-17T03:31:00.003-07:00'
author: JM
tags:
- reverse shell
- RubyScript2Exe
- ruby
- ocra
- undetectable
modified_time: '2012-11-17T19:53:07.837-07:00'
thumbnail: http://1.bp.blogspot.com/-tCXtuRBul-s/UKdgbOYn-hI/AAAAAAAAAJs/H3V9OdOnjxE/s72-c/Screen+Shot+2012-11-17+at+8.00.30+PM.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-8958651965408973192
blogger_orig_url: http://www.pwnage.io/2012/11/ruby-reverse-shell.html
---

I've been working a lot with ruby lately, and having such a high level language makes so many tasks very simple. I've made up a primitive command execution reverse shell, as you can see below:

[![](http://1.bp.blogspot.com/-tCXtuRBul-s/UKdgbOYn-hI/AAAAAAAAAJs/H3V9OdOnjxE/s640/Screen+Shot+2012-11-17+at+8.00.30+PM.png)](http://1.bp.blogspot.com/-tCXtuRBul-s/UKdgbOYn-hI/AAAAAAAAAJs/H3V9OdOnjxE/s1600/Screen+Shot+2012-11-17+at+8.00.30+PM.png)

It takes in connection arguments and connects to a remote host. I just use netcat to receive the connection:

[![](http://2.bp.blogspot.com/-KdO0wx59WQM/UKdV0PgtRGI/AAAAAAAAAI8/CECE8JyeY0Q/s640/Screen+Shot+2012-11-17+at+7.15.09+PM.png)](http://2.bp.blogspot.com/-KdO0wx59WQM/UKdV0PgtRGI/AAAAAAAAAI8/CECE8JyeY0Q/s1600/Screen+Shot+2012-11-17+at+7.15.09+PM.png)

Since most things run ruby today, this could be very useful in most penetration testing situations, however, I wanted something that could be used in environments such as Windows. I looked into [RubyScript2Exe](http://www.erikveen.dds.nl/rubyscript2exe/) which provides packaging of a ruby interpreter into a binary executable. It actually does not work with the current version of ruby, so I kept digging and found a gem called [ocra](http://ocra.rubyforge.org/) which only works under Windows. So I installed ruby on my Windows 7 VM, grabbed the gem and ran ocra on my script:

[![](http://3.bp.blogspot.com/-SXqoz2-FR4o/UKda-l9xK_I/AAAAAAAAAJM/JGKjjn-Zg9s/s640/Screen+Shot+2012-11-17+at+7.34.34+PM.png)](http://3.bp.blogspot.com/-SXqoz2-FR4o/UKda-l9xK_I/AAAAAAAAAJM/JGKjjn-Zg9s/s1600/Screen+Shot+2012-11-17+at+7.34.34+PM.png)

So this was perfect and exciting! Then here's the end result:

[![](http://1.bp.blogspot.com/-eXdKhMstwdg/UKdf4kDbhyI/AAAAAAAAAJk/N2XxmW5zZ0Y/s640/Screen+Shot+2012-11-17+at+7.57.55+PM.png)](http://1.bp.blogspot.com/-eXdKhMstwdg/UKdf4kDbhyI/AAAAAAAAAJk/N2XxmW5zZ0Y/s1600/Screen+Shot+2012-11-17+at+7.57.55+PM.png)

[![](http://2.bp.blogspot.com/-cKJS4gWDXYs/UKdfB5ZgxoI/AAAAAAAAAJc/GRqIswL_lv0/s640/Screen+Shot+2012-11-17+at+7.54.15+PM.png)](http://2.bp.blogspot.com/-cKJS4gWDXYs/UKdfB5ZgxoI/AAAAAAAAAJc/GRqIswL_lv0/s1600/Screen+Shot+2012-11-17+at+7.54.15+PM.png)

Since this has not been done before, and is custom code, we have a fully undetectable reverse shell binary!:

[![](http://1.bp.blogspot.com/-HBGkSIR-f30/UKdnGAQCraI/AAAAAAAAAKA/6XTyCABgSkQ/s640/Screen+Shot+2012-11-17+at+8.26.27+PM.png)](http://1.bp.blogspot.com/-HBGkSIR-f30/UKdnGAQCraI/AAAAAAAAAKA/6XTyCABgSkQ/s1600/Screen+Shot+2012-11-17+at+8.26.27+PM.png)

I'll be expanding on this further, I'll start looking into some file management capabilities, and more advanced shell functions as well as the possibility of encrypting the shell traffic to avoid detection.

Thanks for reading!

Update: Did some searching this morning and found that [secjohn](https://github.com/secjohn/ruby-shells/blob/master/revshell.rb) on github did something extremely similar, he even has a sleep on the reverse function for when it fails. He also mentions ocra! Pretty cool.

~Josh
