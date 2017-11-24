---
layout: post
title:  "S is for Security"
date:   2017-08-26 9:00:00 -0500
categories: server admin
---
Google's gMail security is so amazing. I've gotten warnings before but today's was like, wow. 

I'm working on my app and testing an email function for which I was using a temporary gMail account. I uploaded the file to (what was until a few minutes ago) a public GitHub repo with the address/username and password fully visible. 

In less than a couple minutes, the red band across the top of my gMail page appeared, telling me somebody in the Phillipines had my password and just tried to get into my account! Immediately, I ...

* Changed the password to that gMail account
* Turned off "Allow less secure apps" (I had it on while troubleshooting)
* Added my cell phone number to that gMail account for recovery purposes
* Upgraded to a paid GitHub account and made that repo private
* Changed the database user's password for that app
* Setup two-factor authentication for my host's control panel

November 24, 2017

I can't believe I haven't updated this post since I wrote it three months ago.

Just renewed my Let's Encrypt certificates and verified their status per Step #5 in Digital Ocean's ["How to Secure Apache with Let's Encrypt on CentOS 7"][letsencrypt] tutorial. That super-easy verification gave me an awesome report of all the things it tested and how my site (or just the certificates or both?) did. For example, which attacks my site is and is not vulnerable to. 

[letsencrypt]:
https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-centos-7