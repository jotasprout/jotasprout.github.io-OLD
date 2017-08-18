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

yum update

install

start

enable

secure installation

* enter/set root pw
* remove anonymous users
* disallow remote login
* remove test db
* reload privs

Yer done.