---
layout: post
title:  "P is for PHP"
date:   2017-08-02 10:00:00 -0500
categories: server admin
---
Installing and configuring PHP7.

Why v7?
Because it's better. "Better" includes "more secure" and "way faster."

But what if I know 5.* and don't want or have time to learn 7?
All your current code will still works and all of your current PHP knowledge still applies to everything we'll do.

Install PHP
`some command`

Testing AKA Making Sure It Works
In the web root (`/var/www/html/` in both CentOS and Ubuntu), create the following file containing the following script.

`sudo vi /var/www/html/info.php`

When it's open, 
`<?php phpinfo(); ?>

Open that in your browser using
`http://your_server_IP_address/info.php`

There are some security-related commands and features you may read or hear about that you don't need to worry about because you are a rockstar and installed PHP7 instead of an earlier version. One thing we do need to take of, however, is preventing PHP from displaying some of that information to the outside world. The more information crackers have, the more dangerous they are. 

Security Through Obscurity

There's no reason to keep this file once you're finished with it, so deleting it completely is a good idea.

`sudo rm /var/www/html/info.php`