---
layout: post
title:  "E is for Email"
date:   2017-08-25 10:00:00 -0500
categories: server admin
---
Moving from shared to VPS, I was really afraid I had to make the choice between not having a professional looking email address using my domain name or paying far more than I'm willing to add email to my services.

It looks like this is going to be super easy and I only need to install one or two applications.

* postfix is an MTA and provides the SMTP service
* mailx 
* Dovecot is an MDA and provides the POP3 and IMAP services
* Apache and SquirrelMail provide the Webmail service

Okay, that actually worried me so I'm reading this tutorial instead ...

https://www.tecmint.com/setup-postfix-mail-server-in-ubuntu-debian/

And this one ... 

https://www.digitalocean.com/community/tutorials/how-to-configure-a-mail-server-using-postfix-dovecot-mysql-and-spamassassin

This one is being placed toward the top

http://www.linuxmail.info/

This also looks comprehensive

https://www.linode.com/docs/email/

This looks really nice

http://www.krizna.com/centos/setup-mail-server-centos-7/

And this really good article about why you don't want to do this explaining a bunch of stuff

https://www.digitalocean.com/community/tutorials/why-you-may-not-want-to-run-your-own-mail-server

Like the first one that worried me, this next one also says I have to change my hostname to mail.whatever.com ... I don't really want to do that, correct? Do these tutes imply I need to use a separate server for mail?

https://hostpresto.com/community/tutorials/how-to-setup-an-email-server-on-centos7/

This one has PostfixAdmin in addition to Postfix and, like others, chooses SQLite instead of MySQL ... 

https://www.rosehosting.com/blog/how-to-set-up-a-mail-server-with-postfixadmin-on-centos-7/

