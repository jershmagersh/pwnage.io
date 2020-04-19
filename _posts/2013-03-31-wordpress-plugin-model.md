---
layout: post
title: Wordpress Plugin Security Model
date: '2013-03-31T14:57:00.000-06:00'
author: JM
tags:
- wordpress
- databases
- hardening
- security
- exploits
- web application vulnerabilities
modified_time: '2013-06-11T07:34:20.847-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-3390762538250437153
blogger_orig_url: http://www.pwnage.io/2013/03/wordpress-plugin-model.html
---

## Intro

Recently I've working with some open source Wordpress plugins to identify some vulnerabilities, and I've noticed some issues I would like to address.

Essentially when you install a wordpress plugin, it becomes a part of your code base, and a part of your database. Wordpress itself has been scrutinized to the extent where it is getting fairly secure (no major vulnerabilities, I guarantee you can disclose a directory by navigating to a default plugin under `wp-content/plugins/*.php` or even navigating to the template directory - this is since most admins leave verbose PHP errors on and these scripts don't know how to handle direct GET requests). The issue is that when a Wordpress installation is made, the default functionality isn't always functional enough for the end user. The result is that additional add-ons - in this case plugins - are searched for, uploaded and added to the website accordingly. Now, once this occurs the plugin makes changes to your install base, and can make reference to your core database for pulling its needed information. This results in expanding your attack surface to the code within that plugin.

## The Issue

Well, so you've installed your nifty plugin that lets you do something awesome with your Wordpress installation. This plugin has been open to the development of a number of individuals, so it has a ton of nifty functionality, has been tested accordingly, and committed to the code base with a version release. It must be okay to add to my website right? It must be secure since so many people use it on a daily bases. Well actually, no. Unless somebody has gone through and done a pentest on the plugin (like I've been doing - and even then there's really no guarantee that all aspects have been covered) then there's no verification that the plugin is secure. Lets take a look at what I mean (from a site which actually uses Wordpress and has been compromised :) - [http://www.exploit-db.com/owned-and-exposed/](http://www.exploit-db.com/owned-and-exposed/) ): [Exploit-db](http://www.exploit-db.com/search/?action=search&amp;filter_page=1&amp;filter_description=&amp;filter_exploit_text=wordpress+plugin&amp;filter_author=&amp;filter_platform=0&amp;filter_type=0&amp;filter_lang_id=0&amp;filter_port=&amp;filter_osvdb=&amp;filter_cve=)

Code execution, SQL injection, XSS, etc. etc. brought on by plugins.

## Working Toward a Solution

Plugin isolation would have to take part on both the development side and the installer (Wordpress user's) side. Currently when plugins conduct database queries through a global variable constructed using this call:

`> wp-includes/load.php:   $wpdb = new wpdb( DB_USER, DB_PASSWORD, DB_NAME, DB_HOST );`

This is then called by the plugin like so (taken from a plugin that I found a vulnerability in recently):

`> $wpdb->query( "DELETE FROM {$wpdb->leaguemanager} WHERE `id` = {$league_id}" );`

This results in the plugins making function calls at the same level of the Wordpress installation itself which makes things like querying the database for the administrative hash possible (via SQL injection or what have you). A proposed theoretical solution would be to segregate these into separate databases, and file permission users/groups. 

## Database Hardening

The main database would copy over needed data to the plugin database in order for the same data to be readily available, but leave sensitive information like wp_login in the main database where its confidentiality/integrity can be maintained (to some extent) by the Wordpress code that it is being queried by. The plugin database would obviously also have permissions restricted so that the main database could not be queried (or else the attacker could query for the database with this information). The result being that the plugin should have all information readily available to it to read/write to, while hardening the database attack surface of the main database.

## Filesystem Hardening

The plugin oriented data would then be set under a unique user/group that would restricted to its own areas for writing/executing within the file system through means such as [http://suphp.org/Home.html ](http://suphp.org/Home.html)that provide execution of PHP scripts segregated to a specific user/group. The result being that if a code execution takes place through the means of a plugin, the attacker would be isolated to that section of the filesystem. Now, when further code can be executed through an environment there is other means of breaking out of these restrictions such as privilege escalation, but this would provide yet another layer of security which needs to be bypassed.

This would provide a 'sandbox' at the database end and web application end that would provide failsafes in the event that a plugin is compromised in your infrastructure. Obviously this isn't the most realistic approach, especially for websites using completely free open source solutions like Wordpress - since they're most likely low on funds etc... which would also result in most like shared hosting environments where the user has no control over database management and/or operating system management.

## Filtering the Bad Stuff

Since the two preceding sections would not address things like reflected/stored XSS etc.. there could also be the alternative of Wordpress having mechanisms to prevent web application oriented attacks.

Wordpress currently provides mechanisms for filtering bad requests, like the class mentioned before contains the function prepare:

`> function prepare( $query, $args = null )`

Which `* Prepares a SQL query for safe execution. Uses sprintf()-like syntax.` Which should be used by plugin devs when making queries, so the query mentioned before should look like:

`> $wpdb->query($wpdb->prepare( "DELETE FROM {$wpdb->leaguemanager} WHERE `id` = {$league_id}" ));`

After rummaging through a large amount of Wordpress code I just want to state that the way Wordpress would be filtering posted content in comments, etc... could also be filtered by a query they're making for XSS requests, and the like.

Theoretically if Wordpress itself had security mechanisms in place for every form of web application vulnerability that could be queried by a plugin in order to secure each application request then plugin vulnerabilities would not exist (as long as the developers chose to use those mechanisms). Those most likely already exist, since global PHP functions should be able to be queried by the plugin applications, but that will have to be saved for another post.

Anyways, there's so many things on the go right now that I have had barely any time to make new posts. I'll be doing more research in the near future but I'm currently engulfed by other commitments. More posts will be coming eventually though :)

Stay safe,

~Josh
