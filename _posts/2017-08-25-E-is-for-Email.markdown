---
layout: post
title:  "E is for Email"
date:   2017-08-25 10:00:00 -0500
categories: server admin
---
Moving from shared to VPS, I was really afraid I had to make the choice between not having a professional looking email address using my domain name or paying far more than I'm willing to add email to my services. I found multiple articles explaining why you shouldn't do this yourself but, rather, just use an inexpensive paid service. I found lots of articles--some very informative and thorough--going through the process of installing and configuring two or six apps and at least a couple of those scared me. But, wow, chapter 4 from Mastering CentOS 7 Linux Server is amazing on this topic and I felt comfortable right away so I'm diving in. Again, I got this great book from the library and am using a free Kindle app to read it. Just renewed the book for another three weeks.

While other resources list several apps we'll be installing, Mastering only mentions (at the outset at least) Postfix. Most, if not all, of the other resources included this app as it seems to come pre-installed on most systems. Postfix is a **M**ail **T**ransfer **A**gent (MTA). A pair of similar tools and acronyms you'll see a lot is **M**ail **S**ubmission **A**gent (MSA) and MDA.

You'll quickly realize, as I did, that email is a surprisingly complex system -- not a single app like you may have formerly considered gMail, Mail, or Outlook.

## Opening Ports So eMail Can Get Through

First, I'll open the several standard email ports (most of which) I've heard of:

* 25 for SMTP
* 465 for Secure SMTP
* 587 for MSA
* 110 for POP3
* 995 for Secure POP3
* 143 for IMAP
* 993 for Secure IMAP

We've used this command before

`sudo firewall-cmd --permanent --add-port=993/tcp`

All of them were rewarded with `success` except for 25 which gave me `warning: already_enabled` which I find odd because `firewall-cmd --list-all` doesn't list it. I include thoughts and experiences like this so, if you run into similar things, you'll know you're not the only one and either it's nothing to worry about or how to respond. 

After you've done all those, 

`sudo firewall-cmd --reload`

## Synchronize Your Watches

I've wanted to say that since watching spy movies as a kid. 

You might not care if your blog posts have the exact time they were written so you never bother to set the correct GMT in your preferences but, with email, that can be pretty important. **NTP** synchronizes our server's time with NTP machines around the world. 

`sudo yum install ntp`

The book, *Mastering*, has `install ntpd` and I wasn't the only one who had to Google why that didn't work. It's a type. See? Like I frequently state on my regular blog, you can never trust documentation. I'm not saying you can never trust documentation, but if something doesn't work--while it's usually our fault--you should always at least consider that, perhaps the documentation is incorrect. After I typed it in correctly, it installed just fine. Why did I try it if it looked funny? Because, for example, sometimes I type `firewall-cmd` with no "d" after "firewall" and sometimes I type `firewalld` so ... after it didn't work, I thought it might be a typo and [this article][this-article] confirmed that. Tecmint, by the way, has been my friend since I started this whole VPS business. 

You can edit the configuration file if you need to but it already has some default servers listed. If you have your own you can add it and comment out any or all of the defaults.

`sudo vi /etc/ntp.conf`

I think we're well used to the cycle of "install, config, start, enable" so ... 

    sudo systemctl start ntpd
    sudo systemctl enable ntpd

I tried just `ntp` for those but that didn't work. `ntpd` did.

Testing is, supposedly, `ntpq -p` but my connection was refused. After Googling, I then tried `ntpstat` and received, "Unable to talk to NTP daemon. Is it running?" Then `ntpdc` gave me a `ntpdc>` prompt at which I typed `peer`  but the connection was, again, refused. Some discussions mentioned ntp requires port 123/tcp so I opened that (no love) followed by 123/udp (which I'd never heard of) as well (still no love). `ntpq -pn` also failed.

And then ... 

The article ["HowTo: Verify My NTP Working Or Not"][how-to] from way back in 2010 explained all those commands but concluded with, "If you are using systemd based system" (Ah-ha!) and gave me this which said everything was okey dokey:

`timedatectl status`

Specifically, it gave me this:

    Local time:         Mon 2017-08-28 09:31:24 EDT
    Universal time:     Mon 2017-08-28 13:31:24 UTC
    RTC time:           n/a
    Time zone:          EST5EDT (EDT, -0400)
    NTP enabled:        yes
    NTP synchronized:   yes
    RTC in local TZ:    no
    DST active:         yes
    Last DST change:    DST began at
                        Sun 2017-03-12 01:59:59 EST
                        Sun 2017-03-12 03:00:00 EDT
    Next DST change:    DST ends (the clock jumps one hour backwards) at
                        Sun 2017-11-05 01:59:59 EDT
                        Sun 2017-11-05 01:00:00 EST

Except it was formatted more nicely in my Terminal. The article then tells you how to fix it if `NTP enabled` is set to `no`.

## Hostname Horror Averted (I Thought)

As you might see in my notes at the bottom regarding the articles I didn't use, I was really worried because many of them said I had to change my hostname which makes total sense and wouldn't have phased me if this server was dedicated to email exclusively. I'm going against a lot of online advice, however, and using my server for both web hosting and as a mail server. Many of those tutorials begin with installing the OS, Apache, etc. so I think they assume you're still doing initial setup but I couldn't be sure and I didn't want to change my hostname and screw up things I'd worked very hard to get functioning. *Mastering* states, clearly and comfortingly (yes, I'm inventing a word), "If we receive a fully-qualified domain name `server.domain` [from the command `hostname -f` in the previous paragraph) we can proceed, where `server` is the host name of our server and `domain` is where it belongs. Otherwise, we need to set one by editing the hostname configuration files."

Whew! So, I'm good. That "if ... then" thing was great. But I was still worried because my `hostname -f` gives me

`roxorsoxor.com` with no `mail` or even `www` in front of it and not the `server` that I'd removed because I ... didn't like it. Now, in my hostname files, I have "roxorsoxor.com" and "roxorsoxor" in one and the other has only "roxorsoxor." For whatever it's worth (I can't help but think of this as well), my Apache config file has 

`ServerName www.roxorsoxor.com:80`

I feel like I should add a prefix to those hostname config files ... and I think you can add multiple but the article I found a couple days ago when I first started wrestling with this mail server business that explained it best ... I can't find it. The rest are kinda confusing. 

I keep thinking I should set up multiple virtual hosts like I discussed in an earlier post. But my site is working now and ... gosh, I just don't want to break stuff and keep moving backward. 

Yep, this is the spot at which I keep getting stuck. All this DNS stuff has been my nightmare since I started. A giant whiteboard and a workspace all my own would SO help keep my work and thoughts organized. Ah, to have my own desk, my own bookshelves ... *heavy sigh* ...

I have a gut instinct that it's all easier than it seems but the conflicting, confusing resources I find cast doubts. 

I just don't know if what I have will work and, if I need to change something, what do I need to change? Can I just plop "server" at the front? Does it have to be "mail"? I have pages of handwritten questions. 

The top "best" tutorial at the bottom simply uses "example.com" with no "host" before it. I just ... does that mean I'm okay? I wish I had at least one person I could ask questions. Do I have to set up an MX record? That's a whole other abyss filled with cans of worm-questions. One resource shows an example where the hostname in the MX record is "@" and the server/domain address actually has "mx" in it!

In my hosts DNS Management it says "these records are also known as sub-domains." Really? Is that really all these are? That would really piss me off. But if I install some webmail app, will it want/need/create a "mail" subdomain and any "mail" subdomain I've created screw with it?

So, the hostname has to be "fully qualified" and one resource says that means it includes the domain name ("roxorsoxor" in my case) but they all refer to the "host" which is that "prefix" so ... the hostname has to have a hostname in it? This reminds me that nameservers refer to nameservers so make sure you register your nameservers before you create the nameservers and, by the way, how to do you register a nameserver that you haven't created? Again, what does that prefix have to be? Can it be "poop.roxorsoxor.com"?

I'm putting "mailtime" for Host Name (two words here, not one ...significant?) and "roxorsoxor.com" for Address (why does it have to be different EVERYWHERE?). Let's see what breaks now and/or later. 

## Installing Postfix and Dovecot

* Postfix is an MTA and provides the SMTP service.
* Dovecot is an MDA and provides the POP3 and IMAP services

The default version of Postfix doesn't support MariaDB, the database app I'm using, so I need to get a version from the CentOSPlus repo. Since we're doing that, we have to make sure our version isn't overridden when updating stuff from regular repos. We'll identify and exclude postfix from updates from the regular repo.

`sudo vi /etc/yum.repos.d/CentOS-Base.repo`

and add `exclude=postfix` at the bottom of `[base]` and `[updates]`.

Now for the actual installation.

    sudo yum --enablerepo=centosplus install postfix
    sudo yum install dovecot mariadb-server dovecot-mysql

## Configuring Postfix

Like other configurations, this is just uncommenting and changing some lines.

*Mastering* says we'll define our mail server hostname. Personally, I thought we already did that. "Define," that is. Maybe I'm already screwing this up but I'm putting ...

`myhostname = mailtime.roxorsoxor.com`

As it turns out, the CentOS 7 Cookbook has oodles of yummy information that Mastering does not including a simple up-front test to see if Postfix is already installed, running, and working. First, you send a local user a message

`echo "this is a test" | sendmail username`

and check the log to see if it worked:

`tail -f /var/log/maillog`

but that last command told me maillog doesn’t exist. Google told me, among other things, try restarting syslog. It turns out, that wasn’t even installed!

A quick

`yum -y install rsyslog`

fixed that …

well, I also …

http://www.itzgeek.com/how-tos/linux/centos-how-tos/setup-syslog-server-on-centos-7-rhel-7.html

but I got the stuff described in this

https://support.plesk.com/hc/en-us/articles/214528345-Mailserver-not-working-Warning-SASL-Connect-to-private-auth-failed

so I did the solutions provided in that and tried again

That didn’t work (see notes) so I uncommented that line and tried this instead (which maybe I should have tried first)

https://www.howtoforge.com/postfix-dovecot-warning-sasl-connect-to-private-auth-failed-no-such-file-or-directory

that totally didn’t work so I found and tried this

https://serverfault.com/questions/628966/dovecot-error-unknown-setting-unix-listener

and now dovecot works so I’ll try the test mail message again

totally worked! So now I’m going to send a message to my gmail account …

KICK-FREAKING-ASS! It worked! It went into my spam folder but it worked! And most of these tutes and such include stuff so your messages don’t go to spam and you don’t get blacklisted so I am on my way!

## Testing dovecot and Stakeout Registration with a New User

Just created a new user so I can rebuild/recreate the email registration component of Stakeout. Even if it goes to spam, I can click the link and register new users in Stakeout again and we're a major step closer to being back up to speed. I need to get back to where I was at the old host so I can start making improvements and adding new features. 

These errors are from the app and php:

    SMTP -> ERROR: Failed to connect to server: php_network_getaddresses: getaddrinfo failed: Name or service not known (0)
    SMTP Error: Could not connect to SMTP host. 

The app still tells me it sent an email to the address in the registration form so I need to make sure I add some code that has some more conditions for that. At present, it says it sent the email because of two conditions:

1. It successfully added a tentative user to the app
2. It at least tried to send an email

I need to add a condition that checks for successful SMTP shizzle.

When I saw the errors I remembered something I (think I) read--you can only send messages to external addresses from locally.

Each time I test this, I have to go into the table that holds the user list and delete that new, temporary user so I can try adding it again from scratch. Checking the table and deleting from it (and logging into and using MySQL/MariaDB) from the command line is delightfully good practice.

## LINKS

[this-article]: https://www.tecmint.com/install-ntp-server-in-centos/
[how-to]: https://www.cyberciti.biz/faq/linux-unix-bsd-is-ntp-client-working/

RESOURCES I ULTIMATELY DIDN'T USE BUT SOME ARE EDUCATIONAL

It looks like this is going to be super easy and I only need to install one or two applications.

* mailx 
* Apache and SquirrelMail provide the Webmail service

THIS one looks like the best

https://www.linode.com/docs/email/postfix/email-with-postfix-dovecot-and-mariadb-on-centos-7

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

Maybe ...

https://www.rosehosting.com/blog/mailserver-with-virtual-users-and-domains-using-postfix-and-dovecot-on-a-centos-6-vps/

GOOD STUFF FOR DA LEARNINGZ

https://www.centos.org/forums/viewtopic.php?t=40974



