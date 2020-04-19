---
layout: post
title: Locking Down E-mails With PGP
date: '2012-12-24T13:40:00.001-07:00'
author: JM
tags:
- OSX
- pretty good privacy
- enigmail
- information security
- security
- encryption
- DSA
- RSA
- DES
- PGP
- SHA
- thunderbird
- infosec
- MD5
- AES
- e-mail
- GPGTools
modified_time: '2012-12-25T00:44:39.269-07:00'
thumbnail: http://4.bp.blogspot.com/-7SXaTHklx34/UNiyyNTtbyI/AAAAAAAAALU/CEcInmIm4aY/s72-c/Screen+Shot+2012-12-24+at+12.53.06+PM.png
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-8312075386858107441
blogger_orig_url: http://www.pwnage.io/2012/12/locking-down-e-mails-with-pgp.html
---

## Overview

PGP (Pretty Good Privacy) is an asymmetric trusted key structure that uses the concept of establishing a "web of trust" between patrons delivering data (in this case I will be discussing it in relation to e-mail). Asymmetric keys coincide with the Public and Private key model, in which data can only be encrypted with one key and decrypted with the other. The result is that a user can distribute his/her public key (which will then be public knowledge) without compromising authentication/integrity and confidentiality of the data being exchanged.

### Digital Signatures

Digital signatures provide a method of establishing who the message was sent from (authentication/authenticity) and integrity. This is accomplished through a digital signature which is created using the sender's private key and a hashing algorithm (typically SHA-1). A checksum is produced using a modern one-way, fixed length, cryptographic hash cipher, such as SHA-1, SHA-256, MD5 etc... which is then encrypted with the sender's private key. A digital signatures is then appended to the message being sent. When received it is decrypted with the sender's public key (which is obviously available to the receiver), and the same cryptographic hash cipher is used to produce a checksum from the same message. If these checksums match, then integrity has been held during transmission, and the message is authenticated through only being able to decrypt the message with the sender's public key. 

### Confidentiality

Confidentiality is provided through generation of a random key to be used with a modern symmetric cryptographic cipher. The data being sent is then encrypted using the random symmetric key with the cipher, and the random generated key is then encrypted with the receiver's public key. Since the receiver's private key should not be known to anyone except for him/her, the symmetric key can be decrypted using the private key, which is then used by the receiver to decrypt the original message.

### Web of Trust

In more recent developments there have been established certificate authorities for distribution and acquisition of official public keys (common to the method that is used to distribute SSL certificates online). Other examples are third parties vouching that you are in fact the communicating entity in question. An example I can give of this is the [http://keyserver.pgp.com/vkd/GetWelcomeScreen.event](http://keyserver.pgp.com/vkd/GetWelcomeScreen.event) public key server which requires e-mail verification of associated public keys. Once verified your PGP public key will be distributed through this server.

The Web of Trust is essentially users acquiring keys from trusted sources which are then added to his/her key ring to establish trusted links between communicating entities. This would typically not involve a CA, and thus semi-ambiguous level of trust is assigned to acquired keys. For instance, I will be posting my public key for infosec4breakfast@gmail.com through this post, since only I possess access to this account through multiple security measures, this will most likely be a trusted vector of receiving my public key. GPGTools which is what I have installed on OSX (which I will be talking about later) also has a PGP public key distribution server that can be used to acquire and distribute user's public keys. A "web" is established multiple communicating entities are established of having certain keys. An established level of trust can be assigned to keys in order to provide a level of trust for that key in question.

Once a key ring contains the public key for a user, then confidential encrypted information can be sent to the user in question, as well as being able to maintain authentication and integrity of received messages from that user.

## GPGTools

[GPGTools](https://www.gpgtools.org/) is a great and simplistic way to set up PGP keys to be used with your e-mail address on OSX. Other PGP based tools are available for all other OS's such as gnuPG and numer of others. I'll be covering GPGTools in this post since this is what I'm using. To get started check out this [tutorial](http://support.gpgtools.org/kb/how-to/first-steps-where-do-i-start-where-do-i-begin). Once you have GPG installed, and you've generated your super secure keys, you should distribute your public key to some distribution servers so people can get ahold of it. First, use the built-in functionality in the GPG Keychain Access app:

[![](http://4.bp.blogspot.com/-7SXaTHklx34/UNiyyNTtbyI/AAAAAAAAALU/CEcInmIm4aY/s640/Screen+Shot+2012-12-24+at+12.53.06+PM.png)](http://4.bp.blogspot.com/-7SXaTHklx34/UNiyyNTtbyI/AAAAAAAAALU/CEcInmIm4aY/s1600/Screen+Shot+2012-12-24+at+12.53.06+PM.png)

This will provide a copy of your public key to the GPG key server, which can then be searched for by others using GPG clients, and can then be added automatically to keychains when selected by others.

For further key distribution, export your public key to an .asc file to your local filesystem:

[![](http://1.bp.blogspot.com/-I-vN1uDuYkk/UNizJbGa_KI/AAAAAAAAALc/SfEon-Go5jQ/s640/Screen+Shot+2012-12-24+at+12.54.43+PM.png)](http://1.bp.blogspot.com/-I-vN1uDuYkk/UNizJbGa_KI/AAAAAAAAALc/SfEon-Go5jQ/s1600/Screen+Shot+2012-12-24+at+12.54.43+PM.png)

Check it out:

[![](http://3.bp.blogspot.com/-CyKsNvnSOtM/UNizryI7H3I/AAAAAAAAALk/6WOAjYRFIyU/s640/Screen+Shot+2012-12-24+at+12.57.21+PM.png)](http://3.bp.blogspot.com/-CyKsNvnSOtM/UNizryI7H3I/AAAAAAAAALk/6WOAjYRFIyU/s1600/Screen+Shot+2012-12-24+at+12.57.21+PM.png)

Now you can distribute that to your friends, and some public key servers. Some examples are the pgp.com public key server and the [MIT public key server](http://pgp.mit.edu/)

Here I'll be submitting my key to the MIT server like so:

[![](http://3.bp.blogspot.com/-GfxPNyo2MuU/UNi2UyIyXNI/AAAAAAAAAL0/Or8n5mjA9mM/s640/Screen+Shot+2012-12-24+at+1.07.16+PM.png)](http://3.bp.blogspot.com/-GfxPNyo2MuU/UNi2UyIyXNI/AAAAAAAAAL0/Or8n5mjA9mM/s1600/Screen+Shot+2012-12-24+at+1.07.16+PM.png)

Then it can be looked up through providing a string to the "Extract a Key" search:

[![](http://3.bp.blogspot.com/-uN-2vwBAnpU/UNi2kKT81II/AAAAAAAAAL8/FtHaVn_RH0Y/s640/Screen+Shot+2012-12-24+at+1.08.19+PM.png)](http://3.bp.blogspot.com/-uN-2vwBAnpU/UNi2kKT81II/AAAAAAAAAL8/FtHaVn_RH0Y/s1600/Screen+Shot+2012-12-24+at+1.08.19+PM.png)

Now since you have distributed your key to a number of servers, users can now send you encrypted e-mail. You'll want to get familiar with some public avenues of attaining public keys of others to verify their authenticity and be able to send encrypted e-mail to other users.

The following is an example of using the gpg command line tool with an established key ring:

[![](http://4.bp.blogspot.com/-C4wyXlQmo2w/UNi38PyKxjI/AAAAAAAAAMM/5GgT9snhuoI/s640/Screen+Shot+2012-12-24+at+1.14.40+PM.png)](http://4.bp.blogspot.com/-C4wyXlQmo2w/UNi38PyKxjI/AAAAAAAAAMM/5GgT9snhuoI/s1600/Screen+Shot+2012-12-24+at+1.14.40+PM.png)

Obviously it's completely encrypted, so I'm going to use the gpg command line tool to decrypt this since I have the public key of the sender in order to verify the digital signature, and I have my private key which will be used to decrypt the message that was encrypted with my public key:

[![](http://1.bp.blogspot.com/-dkZIByd73rQ/UNi5hXk5UJI/AAAAAAAAAMk/OvqN4j8xWHA/s640/Screen+Shot+2012-12-24+at+1.21.14+PM.png)](http://1.bp.blogspot.com/-dkZIByd73rQ/UNi5hXk5UJI/AAAAAAAAAMk/OvqN4j8xWHA/s1600/Screen+Shot+2012-12-24+at+1.21.14+PM.png)

Decrypting the message:

[![](http://2.bp.blogspot.com/-bo8HuFM_LlE/UNi5whw4ReI/AAAAAAAAAMs/oNCHkyxaVtQ/s640/Screen+Shot+2012-12-24+at+1.21.36+PM.png)](http://2.bp.blogspot.com/-bo8HuFM_LlE/UNi5whw4ReI/AAAAAAAAAMs/oNCHkyxaVtQ/s1600/Screen+Shot+2012-12-24+at+1.21.36+PM.png)

As you can see this message was both signed by myself, and encrypted with the infosec4breakfast@gmail.com public key. The plain text message is then saved to the file called "message".

## Enigmail Thunderbird Add-on

Now that we've had fun with the command line, this isn't always the most efficient way to read/write e-mail. I use the [Thunderbird](http://www.mozilla.org/en-US/thunderbird/) for all my mail client needs. There's a terrific add-on called [Enigmail](https://addons.mozilla.org/en-US/thunderbird/addon/enigmail/) that will work with with thunderbird and your currently established PGP key ring to send and receive encrypted e-mail. That is what I used to send e-mail to the encrypted e-mail to the infosec4breakfast@gmail.com account.

Just to show you how easy it is, you just have to enable the encryption options for your e-mail:

[![](http://4.bp.blogspot.com/-hCI36cJF2RM/UNi78woVZeI/AAAAAAAAAM8/KFGxjHpnL7I/s640/Screen+Shot+2012-12-24+at+1.32.33+PM.png)](http://4.bp.blogspot.com/-hCI36cJF2RM/UNi78woVZeI/AAAAAAAAAM8/KFGxjHpnL7I/s1600/Screen+Shot+2012-12-24+at+1.32.33+PM.png)

Everything is then taken care of for you! It will also automatically decrypt messages received and display that the message is signed by the sender!

Well, that pretty much covers all things PGP associated with OSX and clients. I highly recommend using as many layers of security as you can throughout all of your communication practices. It's so easy now, so why not? I want to wish everyone a very merry Christmas and happy holidays!

Be safe!

-Josh
