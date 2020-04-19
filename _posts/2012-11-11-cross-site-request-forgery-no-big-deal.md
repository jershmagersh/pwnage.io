---
layout: post
title: Cross-site Request Forgery, No Big Deal? Think Again.
date: '2012-11-11T01:05:00.000-07:00'
author: JM
tags:
- Web Application Security
- sql injection
- web application vulnerabilities
- cross-site request forgery
- csrf
- php
modified_time: '2012-11-12T04:05:14.402-07:00'
thumbnail: http://2.bp.blogspot.com/-0ZHA0j1d2FA/UJ9WNhTOlCI/AAAAAAAAAIU/OxmfYbw84jU/s72-c/Screen+Shot+2012-11-11+at+5.38.56+PM.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-2535760905601246684
blogger_orig_url: http://www.pwnage.io/2012/11/cross-site-request-forgery-no-big-deal.html
---

  

## Intro

Recently I've been delving into research of a number of popular web applications in order to find potential security vulnerabilities. So far, the most prevalent and the least checked for are cross-site request forgeries. To make the deal even sweeter, I've been finding CSRF vulnerabilities resulting in more severe attack vectors in parts of applications that are not accessible by regular users. 

## Who needs to worry about that admin side XSS or SQL injection? It's not even available to the public!

Even though a vulnerability is not available to the public, and/or lower privileged users, a request can be crafted to perform a CSRF that makes the admin perform an action to exploit the web application. Is getting administrators or other site users that difficult? Some intriguing media is usually all you need usually, then some nice iframe magic and you can have Admins compromising their own websites all day.

## Example

Just to elaborate, I'm going to show you a basic example. Say we have this input in PHP:

admin_input.php:

[![](http://2.bp.blogspot.com/-0ZHA0j1d2FA/UJ9WNhTOlCI/AAAAAAAAAIU/OxmfYbw84jU/s640/Screen+Shot+2012-11-11+at+5.38.56+PM.png)](http://2.bp.blogspot.com/-0ZHA0j1d2FA/UJ9WNhTOlCI/AAAAAAAAAIU/OxmfYbw84jU/s1600/Screen+Shot+2012-11-11+at+5.38.56+PM.png)

So yeah, super basic, and as you can see all parameters supplied are directly inserted into the SQL statement without being sanitized. This results in the input being vulnerable to SQL injection.

If they have Admin access we're already compromised, right? Well actually no. Since no Nonce is in place (A number or string only used only once within the context of cryptographic communication, or in this case, with making a request to the established web application) to prevent a CSRF attack from happening the attacker can post news articles (oh awesome :P) using a CSRF attack and/or they can perform an SQL injection on behalf of the Administrator (a lot more awesome). Here is an example of a malicious page that the attacker could have hosted on his site that he would have to get the Administrator in question to visit:

[![](http://3.bp.blogspot.com/-g5d4AJ-5Gq0/UJ9adeXbYNI/AAAAAAAAAIk/IQGBMTQGvKs/s640/Screen+Shot+2012-11-11+at+5.56.58+PM.png)](http://3.bp.blogspot.com/-g5d4AJ-5Gq0/UJ9adeXbYNI/AAAAAAAAAIk/IQGBMTQGvKs/s1600/Screen+Shot+2012-11-11+at+5.56.58+PM.png)

In this scenario the injection would make a new article with the body being the username and password of the administrator (I set it to being id = 1 from users just for fun, but typically it would be the first user in the database). Cool eh?

## There's a Lot More to This

This is by no means the reason that CSRF attacks are a lethal threat. This is since requests could be made to add further users, administrators, delete data, and the like; I just wanted to present some of my current findings. In most situations where not having a token (nonce) seems harmless, it really isn't, as you'll be seeing with some vulnerabilities I'll be posting next month.

Thanks for reading!

~Josh
