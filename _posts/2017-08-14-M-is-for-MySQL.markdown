---
layout: post
title:  "M is for MySQL"
date:   2017-08-14 10:00:00 -0500
categories: server admin
---
With MySQL, you create, use and manage databases. 

Any shared server should already have MySQL installed and any host worth paying more than a few cents for allows you to create as many databases as you'd like.

I couldn't help but notice that every CentOS tutorial that included LAMP setup specifically used MariaDB so I researched it (I scanned two sites) and decided that's what I'll use. You can read for yourself here:

* https://seravo.fi/2015/10-reasons-to-migrate-to-mariadb-if-still-using-mysql
* https://mariadb.com/kb/en/mariadb/mariadb-vs-mysql-features/

According to their excellent (and visually attactrive) documentation, it uses the exact same commands -- it's all SQL -- so there's no learning curve.

So, let's get to installing.

My goodness but their documentation is pretty (sorry). 

## Update your Yummy Goodness

    yum update

## Install

`sudo yum install mariadb-server mariadb`

## Start and Enable

`sudo systemctl enable mariadb.service && systemctl start mariadb.service`

Do I need Mastering's `sudo yum install epel-release`?

## Secure installation

`mysql_secure_installation`

This is a command with which you shouldn't use `-y` (which automatically answers "yes" to questions during installation). The following help in hardening security for your databases.

The first question is for a database root administrator password. Since this is a new installation, there is no password for the root administrator so just press Enter/Return. You then set a new, powerful password.

* enter/set root pw
* remove anonymous users
* disallow remote login
* remove test db
* reload privs

Yer done. 

## Testing and More Security

The following command will "run the CLI interface" (what does that mean?).

`sudo mysql -u root -p`

If you don't want to enter the password every time, put it in a file kept in the home directory. The home directory (outside of your public html directory) is where you should keep all passwords. I discuss this further in lessons when we start building an actual site and app. So, in a file (such as?) `~/.my.cnf` add:

`[mysql] \npassword=password`

## Creating a New User

Why not just use root? For the same reason we disabled root ssh login during our initial setup. We want it to be as difficult as possible for bad guys and that means we not only use a different password for everything but, if we can, a different user for everything. Seriously consider having a different user that administers each database. That way, even if a bad guy gets one set of credentials, they can only screw with one database. Go even further. Have a different user for different purposes in each database. One that is administrator for creating tables, adding columns and so forth and a separate one that can only update those tables and add rows. So, even if a bad guy gets some end user's creds (a member of your site that uses your software) and somehow breaks into the back end, they might be able to see ("read") what is there but they still can't break anything. 

### Sources

While I used many sources, many of them contained exactly the same information and/or commands so I'm only listing the ones that contained additional information.

* Mastering CentOS 7 Linux Server by 