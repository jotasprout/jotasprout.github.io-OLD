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

Grant Yourself SuperPowers
In CentOS, users in the "wheel" group have "super user" privileges so I'm going to add this new account--that I'll use from now on instead of using root--to the "wheel" group.
`gpasswd -a user1 wheel`

If you're using another OS, you'll have to do something else. 

Instructions for Ubuntu or whatever ...

Instructions for Public Key Authentication

Instructions for Disabling Root

Instructions for Setting Up Two-Factor Authentication
Few things are as secure as two-factor authentication and this method is also both free and easy. For Ubuntu, it's one, short line of code. For CentOS--what I'm using--it's a wee bit (but not much) longer. 

Installing Google Authenticator