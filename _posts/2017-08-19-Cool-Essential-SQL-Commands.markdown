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

## Keys

* PRIMARY
* FOREIGN
* UNIQUE
* INDEX
