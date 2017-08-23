---
layout: post
title:  "Initial Server and Security Setup"
date:   2017-08-01 10:00:00 -0500
categories: server admin
---
We're going to do two things. First, make sure we can access it and do whatever we want. Second, make sure nobody else can access it.

Initially, you'll login as root--then, neither you nor anyone else should do that again. Login in via this humble command
`ssh root@Ye.Olde.IP.Address`

Until your domain is all setup and configured, you can use your IP address as shown above.

Create an obscenely unique new username with a ridiculously complicated password. Here, my new username is "user1."
`adduser user1
passwd user1`
It'll ask you for your pw then ask again in case you have fumble fingers.

### Grant Yourself SuperPowers

In CentOS, users in the "wheel" group have "super user" privileges so I'm going to add this new account--that I'll use from now on instead of using root--to the "wheel" group.

`gpasswd -a user1 wheel`

If you're using another OS, you'll have to do something else. 

## Instructions for Public Key Authentication

Any human or computer -- and maybe even a monkey given enough time -- can guess your password eventually. If you use a pair of keys, however, only the person with the key can get in. So, in short: don't use passwords--use keys.



## Instructions for Disabling Root

Give ever user a limited amount of power. That way, even if a bad actor gets that user's credentials, they're limited in the damage they can do. 

Making it impossible for anyone--even you-- to log in as root means bad guys (and gals, of course, #LadyGoatsCanBeCrackers) can only log in as one of those limited users.

Start by opening the ssh config file.

`sudo vi /etc/ssh/sshd_config`

Find the following two lines (they aren't together), uncomment them, and change their values to no. You'll need to press "i" to get into Insert mode.

	PermitRootLogin				no
	PasswordAuthentication		no

Press Esc to get back into command mode and press ":x" to save and close the file.

Reload SSH to apply your changes.

`sudo systemctl reload sshd`

## Firewall Installation and Management

Install it. 

Start it.

`sudo systemctl start firewalld.service`

Sometimes you'll see commands such as the above without ".service" because CentOS (with some exceptions) adds it automatically. 

Allow it to start with each reboot.

`sudo systemctl enable firewalld.service`

If you make a mistake, you can always reboot your server and these changes are removed unless you used the `--permanent` argument (or is it an "option"?). That also means you can test all of these without fear.

Allow public web traffic. The second line is for SSL if you're using that (and you should).

	firewall-cmd --zone=public --add-service=http
	firewall-cmd --zone=public --add-service=https

After testing, you'll type all that in again but add `--permanent` for each.

Also allow specific ports.

`firewall-cmd --zone=public --add-port=80` for your web traffic

## Two-Factor Authentication
Few things are as secure as two-factor authentication and this method is also both free and easy. For Ubuntu, it's one, short line of code. For CentOS--what I'm using--it's a wee bit (but not much) longer. 

Installing Google Authenticator

Instructions found at:

https://www.a2hosting.com/kb/getting-started-guide/accessing-your-account/enabling-two-factor-authentication-for-ssh
