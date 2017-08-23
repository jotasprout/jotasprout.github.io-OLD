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

They have excellent (and visually attactrive) documentation which you might want to check out because MariaDB uses a slightly different syntax so there's a wee learning curve.

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

* The first question is for a database root administrator password. Since this is a new installation, there is no password for the root administrator so just press Enter/Return. 
* Maria then asks if you want to set a new root password. Of course you do.
* Remove the default anonymous users? Of course. Yes.
* Disable remote root login? Absolutely. Yes.
* Remove the test database included with the install? Yes.
* Reload Privilege tables? That means "apply everything I just did." Yes. 

Yer done. 

## Testing and More Security

The following command will "run the CLI interface" (what does that mean?).

`sudo mysql -u root -p`

SIDEBAR START

If you don't want to enter the password every time, put it in a file kept in the home directory. The home directory (outside of your public html directory) is where you should keep all passwords. I discuss this further in lessons when we start building an actual site and app. So, in a file (such as?) `~/.my.cnf` add:

`[mysql] \npassword=password`

SIDEBAR STOP

## Creating a New User

Why not just use root? For the same reason we disabled root ssh login during our initial setup. We want it to be as difficult as possible for bad guys and that means we not only use a different password for everything but, if we can, a different user for everything. Seriously consider having a different user that administers each database. That way, even if a bad guy gets one set of credentials, they can only screw with one database. Go even further. Have a different user for different purposes in each database. One that is administrator for creating tables, adding columns and so forth and a separate one that can only update those tables and add rows. So, even if a bad guy gets some end user's creds (a member of your site that uses your software) and somehow breaks into the back end, they might be able to see ("read") what is there but they still can't break anything. 

While we're logged in as root, we'll create a database 

`CREATE DATABASE <my-new-database> CHARACTER SET utf8 COLLATE utf8_general_ci;`

If you're switching to MariaDB from MySQL, for example, backward-compatibility is a concern, so you might want to use `latin1` with `latin1_swedish_ci`. If this is all new, which mine is, we're going to use utf8.

SIDE-THOUGHT: How does utf8 relate to or affect my PDFtk shizzle?

Then we create a user with access to that database

`GRANT ALL ON <my-new-database>.* TO '<username>'@'localhost' IDENTIFIED BY '<password>' WITH GRANT OPTION;`

Granting `ALL` creates our new  admin for this database. This is the basic template for creating users:

`GRANT [type of permission] ON <database-name>.<table-name> TO '<user-name>'@'<host-name>';`

The commands/options/permissions you can grant a user are as follows:

* `ALL` means, you know, all. Anything and everything.
* `CREATE` can create new tables and/or databases (yeah, I know, hold on, we'll get there)
* `DROP` deletes tables and/or databases (dude, seriously, chill, it's coming)
* `INSERT` creates/inserts new records or "rows" in a table (see? I told you to chill)
* `SELECT` means request, query, ask for stuff ... like, "Computer, who accessed the mainframe last Tuesday" or "List all albums by David Bowie"
* `UPDATE` can change the data in a row or info about tables such as "Computer, change the artist name from Cat Stevens to Yusuf Islam."

You can get really specific with users and strictly limit what they can do. If customers/members using your app need to have different levels of permissions, those different types of accounts should use different database users. Is that a whole bunch of extra work? Yes and it also means you won't be in the news like Sony and your customers won't be go down with your ship like ... whatever that online whorehouse was called. 

SECURITY TIP: Delete old user accounts. Delete testing accounts when you're finished with them. But you need test users, why recreate them, right? Well, then create a group with the necessary permissions and just add/remove users as needed. WAIT, are their groups in SQL like that?

This kinda refreshes the database so it knows about the new user

`FLUSH PRIVILEGES;`

Exit so we can test as a new user

`EXIT;`

Log into the database as the new user

`mysql -u <username> -p`

You'll get the prompt at which you'll try a command and exit because we're done now.

    SHOW DATABASES;
    EXIT;

### Sources

While I used many sources, many of them contained exactly the same information and/or commands so I'm only listing the ones that contained additional information.

* Mastering CentOS 7 Linux Server by Bhaskarjyoti Roy
* CentOS 7 Linux Server Cookbook by Oliver Pelz