---
layout: post
title: Hostels & Supporting Companies Not Prioritizing IT Security
date: '2012-12-03T00:59:00.002-07:00'
author: JM
tags:
- infosec
- ettercap
- arp poisoning
- windows
- hostels
- exploits
- travelling
- windows xp
- IT security
modified_time: '2012-12-03T04:58:26.678-07:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-4712822523935035005
blogger_orig_url: http://www.pwnage.io/2012/12/hostels-supporting-companies-not.html
---

## Intro

Recently I went up to Cairns for a diving trip with some friends (I'm Open Water certified). It was really fantastic and saw tons of cool things on the Great Barrier Reef. Along the way I noticed some things that troubled me. It all comes back to the problem of nobody putting in the extra time, or money to sustain a secure environment for travelling backpackers at establishments like hostels.

## SSL

I'm going to leave names out of this, because I don't want to point fingers at anyone. During my reservation process online I went to the Hostel's website that I was going to be staying at. I navigated to my wanted booking dates (this is a very popular hostel so I wanted to book ahead of time) and chose which ones I wanted. On the payment page where Credit Card details were entered I checked (as I always do) and there was no SSL involved at this point. Typically a site will redirect you to an SSL (HTTPS) side of the site when it comes to payment time, obviously to have payment data encrypted as it travels across the internet to the server in question. I proceeded to hit the site with https://site.com to see if I could be under SSL the whole time, but their SSL certificate was expired so I didn't want to go this route. I ended up using a site that provides bookings across multiple hostels to book at the same one which used an SSL portal to submit payment.

When I arrived at the hostel I told them this, so hopefully whomever runs their site will fix it, and managed to avoid some overhead fees for helping them out ;)

## Public Machines

At every hostel they have public computers for backpackers and the public to use. Here they use ones that I've seen across my hostel ventures (which shall still remain nameless). This is obviously a well established company since it is widely used, and distribute a large amount of machines accordingly.

During my stay I was checking some things, even though I hate using publicly used machines I have some mitigations in place in case of a compromise like two factor authentication, and I know these machines use a freshly booted image each time they're restarted so I felt alright (at first). I proceeded to look at some version numbers of the machine being used, they use Firefox 6.0 (super old) and Windows XP SP2 (super old), and a ton of other old software. Obviously from the version numbers you can guess that these are vulnerable to a wide range of exploits and had Windows automatic updates disabled, resulting in a completely vulnerable machine. You could also download and execute anything you wanted, being local privilege exploits, or what have you. 

Since the images are booted fresh every time this is fine and dandy isn't it? Well the machines that these were on actually had bootable devices enabled. If anyone was so inclined they could boot into anything they wanted, since there was also access to USB ports on every single machine. The first thing that came to mind for me was having one rogue machine on this network that could do something like ARP poisoning, with something like [ettercap](http://ettercap.sourceforge.net/), redirect user browsers to a malicious browser exploit page with something like [Browser Autopwn in Metasploit](http://www.offensive-security.com/metasploit-unleashed/Browser_Autopwn), and compromise each box when a browser was used, once this was done then un-poison them and head on your way. Obviously further MITM could take place if needed as well. This is just a simple example but you could really do anything with a local rogue machine on the network.

Other things could involve editing the bootable XP image directly with a bootable linux distro to accompany bootable malware, depending on what they use. There's so many different angles that could be taken it's scary, and hopefully none of this happens while they have these machines in place.

## Afterthoughts

Hostels are ran by non-tech oriented people, and want to make the most bang for their buck due to their low pricing. I understand this, however, the company providing the machines being used should have locked these things down. The old images are one thing, but disabling boot options is not rocket science. It would have been one extra step. People using Hostels are usually backpackers, and/or people in the lower income bracket. The last thing they would need is unknown credit card charges, or things getting locked down by their bank when they're nowhere close to home. I'll be contacting the machine vendors to see their thoughts, and if anything can be done. I'll update this accordingly.

-Josh
