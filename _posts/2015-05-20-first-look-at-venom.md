---
layout: post
title: First Look at Venom
date: '2015-05-20T21:11:00.003-06:00'
author: JM
tags: 
modified_time: '2015-06-07T15:28:12.600-06:00'
thumbnail: http://4.bp.blogspot.com/--LjbZaso7rI/VV1GM97ed7I/AAAAAAAAAWQ/QJGxlZCKTGU/s72-c/Screen%2BShot%2B2015-05-13%2Bat%2B11.06.35%2BPM.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-6193423361970595817
blogger_orig_url: http://www.pwnage.io/2015/05/first-look-at-venom.html
---

## Intro

For those of you who are not aware this was dropped last week by CrowdStrike: http://venom.crowdstrike.com/ essentially a virtual floppy disk controller (FDC) that was originally from the qemu project which was later adopted by a number of other projects (including QEMU, Xen, KVM, VirtualBox etc.) contains a buffer overflow that can lead to memory corruption within the a running hypervisor using instructions being sent to the FDC. The result being that a prior knowledge of the hosting architecture and structures would be needed, but we'll get more into that later on. The obvious impacts are the largest for data centers, since the only access that is required to exploit the vulnerability is root/system/driver level access on a system running on the hypervisor that would have the ability to send these driver instructions. The advisory goes into many of the impacts, and I suggest you revise them.

## Setup

I'm running OS X so I had to install a Linux VM that then had QEMU installed for simplicity to begin with, which I then installed another Linux QEMU instance (qcow image) on top of this that I would be using to test this vulnerability. I chose to use QEMU as this has had a patch released for it here: [http://git.qemu.org/?p=qemu.git;a=commit;h=e907746266721f305d67bc0718795fedee2e824c](http://git.qemu.org/?p=qemu.git;a=commit;h=e907746266721f305d67bc0718795fedee2e824c) and wanted to run it under Linux for later testing with KVM.

I then saw this come across my twitter feed from HD Moore: [https://marc.info/?l=oss-security&m=143155206320935&w=2](https://marc.info/?l=oss-security&amp;m=143155206320935&amp;w=2) which was the first public PoC. This code imports the Linux IO library, which uses `#define FIFO 0x3f5` to define the IO port constant to write to, sets the I/O privilege level to 3 (which gives you access to all I/O ports) here: `iopl(3);` then uses [outb](http://man7.org/linux/man-pages/man2/inl.2.html) to write the device Read ID to the `FIFO` port, then uses `outb` within a loop to write `10000000` bytes (`0x42` being the ASCII letter `B`). This is my interpretation of the linux kernel code, as you can see my knowledge of this is fairly limited :). I found this article very useful for understanding the PoC code: [http://tldp.org/HOWTO/IO-Port-Programming-2.html](http://tldp.org/HOWTO/IO-Port-Programming-2.html)

## Crash

Unsurprisingly this causes a crash in the application considering we're corrupting so much memory. In my version of QEMU the crash was caused by the rdx register containing `0x4242424242424242` which is obviously a non-existent address, and being dereferenced for a QWORD ptr that is set into rax that is later relatively referenced (`rax+0x18`) for a cmp 0x0, looks like we already have some kind of flow control, although we have no knowledge of relative addresses that are required to point to our overwritten stack address.

After subsequent runs I noticed that the PoC was causing inconsistent results, most likely due to the fact that we were overwriting so much memory that it was affecting a large amount of data stored on the stack and can result in different branches called. A result of another run than the one stated above can be seen here:

[![](http://4.bp.blogspot.com/--LjbZaso7rI/VV1GM97ed7I/AAAAAAAAAWQ/QJGxlZCKTGU/s640/Screen%2BShot%2B2015-05-13%2Bat%2B11.06.35%2BPM.png)](http://4.bp.blogspot.com/--LjbZaso7rI/VV1GM97ed7I/AAAAAAAAAWQ/QJGxlZCKTGU/s1600/Screen%2BShot%2B2015-05-13%2Bat%2B11.06.35%2BPM.png)

## Difficulty Ahead

Working toward a working exploit with modern mitigation techniques (PIE compiled executable, non-executable stack) on the host machine running QEMU would be/is going to be a difficult task for anyone who wants to develop a proof of concept. This would involve memory disclosure to the guest operating system for a PIE/ASLR bypass, and writing a ROP chain to bypass a non-executable stack.

An example of this is a talk that I saw at REcon last year:[ https://www.youtube.com/watch?v=i29bAx6W1uI](https://www.youtube.com/watch?v=i29bAx6W1uI) required the following vulnerabilities to successfully escape from the guest and achieve code execution on an x86 Windows host machine:

• CVE-2014-0981: VirtualBox crNetRecvReadback Memory Corruption Vulnerability

- It’s a write-what-where memory corruption primitive by design, within the address space of the hypervisor.

• CVE-2014-0982: VirtualBox crNetRecvWriteback Memory Corruption Vulnerability

- Another memory corruption primitive by design, within the address space of the hypervisor.

• CVE-2014-0983: VirtualBox crServerDispatchVertexAttrib4NubARB Memory Corruption Vulnerability

- Allows the attacker to corrupt arbitrary memory with a pointer to attacker-controlled data

VUPEN later released a technique to gain reliable code execution with process continuation on a 64-bit Windows 8 host platform using only one of the mentioned vulnerabilities: [http://www.vupen.com/blog/20140725.Advanced_Exploitation_VirtualBox_VM_Escape.php](http://www.vupen.com/blog/20140725.Advanced_Exploitation_VirtualBox_VM_Escape.php) both being great feats in exploit development. Which gives light to the possibility that this memory corruption vulnerability could lead to code execution, although I am still skeptical. CrowdStrike states they'll be releasing further details in the future, and didn't want to provide too much information to avoid seeing exploits in the wild too quickly. We'll see how things progress.

## Update

Looks like they've provided the vulnerability details here: [http://blog.crowdstrike.com/venom-vulnerability-details/](http://blog.crowdstrike.com/venom-vulnerability-details/) much more detailed than the initial public report. Obviously not providing any methods for exploitation as this is just a more detailed vulnerability write-up.
