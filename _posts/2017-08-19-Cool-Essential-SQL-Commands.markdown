---
layout: post
title:  "Cool & Essential SQL Commands"
date:   2017-08-19 14:30:00 -0500
categories: jekyll update
---
How do I get rid of a database if an end user is deleting and/or resetting stuff?

`DROP DATABASE IF EXISTS <yucky-old-database>;`

If I'm running a regular/frequent query to, say, Spotify, and it needs to periodically create new stuff when it finds new stuff:

`CREATE DATABASE IF NOT EXISTS <shiny-new-database>;`

## Logging Into MariaDB and/or MySQL

`mysql -u <username> -p;`

`-u` is for "hey, here comes a username" and `-p` is for "ask for my password." Oh, and never, ever forget the semi-colon. As with other languages, it's the most obvious screwup and the last one you'll remember as you're slowly driven insane.

You'll see a prompt like this:

`[MariaDB [(none)]>`

after which you type commands.

## "Logging Into" a Specific Database

You may be a database user, but you don't have permission to use all databases. To see the databases to which you are assigned:

`SHOW DATABASES;`

To use one of those databases (suppose it's called "comic-books"):

`USE <comic-books>;`

You'll then see the database name replace "none" in the prompt.

`[MariaDB [<comic-books>]>`

## Keys

* PRIMARY
* FOREIGN
* UNIQUE
* INDEX

## CRUD

* INSERT
* SELECT
* DELETE

A database is made of multiple tables. Think of an Excel workbook with multiple pages, each containing a different spreadsheet. The workbook is a database and each spreadsheet is a table. Now, imagine the spreadsheets are connected somehow--the totals from one spreadsheet become the line items in another spreadsheet on a different page for example. Those spreadsheets are related. A relational database is a database in which the tables are related or connected.  

Having said all that, whenever we give our database a command, we must tell it which table we're talking about. In this case, we want a list of all the comic book characters in a table called "characters."

`SELECT * FROM characters`

You're already in the "comic-books" database, so you don't need to specify that.