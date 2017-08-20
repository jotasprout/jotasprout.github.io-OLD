---
layout: post
title:  "Creating A Database"
date:   2017-08-19 15:20:00 -0500
categories: jekyll update
---
First, we need to log into MariaDB (or MySQL, etc.). 

`sudo mysql -u root -p`

After some welcome, version, and copyright info from  MariaDB, I'm asked for my OS sudo password then my database root password. If I answer both correctly, I get the following prompt:

`MariaDB [(none)]>`

after which, I do this: 

`CREATE DATABASE <shiny-new-database> CHARACTER SET utf8 COLLATE utf8_general_ci;`

Then we create a user with access to that database

`GRANT ALL ON <shiny-new-database>.* TO '<username>'@'localhost' IDENTIFIED BY '<secret-password>' WITH GRANT OPTION;`

Granting `ALL` makes that user an admin. Now let's exit and try logging back in with our shiny new user.

`mysql -u <username> -p`

We get MariaDB info if we succeeded. Let's see the databases to which `<username>` has access.

`SHOW DATABASES;`

Use InnoDB as your engine because the default iSAM doesn't allow foreign keys and, verily verily, I say unto you, if you can't use foreign keys, why wouldst thou useth a relational database?

Does MariaDB use MySQLi?

Users come in two flavors: Evil and Stupid. Never, ever trust users. Ever. Always "sanitize" their input.  

Always use UTF-8. Not only is it new and shiny, it's international. 

And more from

https://www.sitepoint.com/mysql-mistakes-php-developers/