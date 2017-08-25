---
layout: post
title:  "P is for PHP"
date:   2017-08-02 10:00:00 -0500
categories: server admin
---
Installing and configuring PHP7.

Why v7?
Because it's better and by "better" I mean "more secure" and "way faster."

But what if I know 5.* and don't want or have time to learn 7?
All your current code will still work and all of your current PHP knowledge still applies to everything we'll do.

## Installing PHP (and the library that supports MySQL)

`sudo yum install php php-mysql`

But wait ... I don't want php-mysql, do I? Isn't there a new one? Or something different for MariaDB?

Does that automatically install v7?

Check to see it installed and the version by typing:

`php --version`

Is the above command correct?

Let's see what we have.

## Testing AKA Making Sure It Works
In the web root (`/var/www/html/` in both CentOS and Ubuntu), create the following file containing the following script.

`sudo vi /var/www/html/info.php`

When it's open, type

`<?php phpinfo(); ?>`

Open that in your browser using

`http://your_server_IP_address/info.php`

Review that and you'll see things I'll refer to soon. 

## Configuration and Security

Open this file for editing

`sudo vi /etc/php.d/security.ini`

Change the following

Does capitalization of values ("off" vs "Off") count? Test this.

We don't want visitors to know anything about our code so we're going to prevent errors from being displayed publicly -- security through obfuscation -- and have them stored in logs kept elsewhere for our eyes only.

    expose_php=Off
    display_errors=Off

    log_errors=On
    error_log=/var/log/httpd/php_scripts_error.log

We also want to disallow remote code execution (EXPLAIN what this means)

`some code goes here`

Mastering recommends installing "the Suhosin advanced protection system" to protecting our PHP "from known and unknown flows" and I don't yet know what that means.

`more code goes here`

If you've already installed Apache, restart it. DOES THE ORDER OF INSTALLATION MATTER?

`sudo systemctl restart httpd`

There are some security-related commands and features you may read or hear about that you don't need to worry about because you are a rockstar and installed PHP7 instead of an earlier version. One thing we do need to take of, however, is preventing PHP from displaying some of that information to the outside world. The more information crackers have, the more dangerous they are. 

REMINDER: Apache needs to know about php index pages

## Security Through Obscurity

There's no reason to keep this file once you're finished with it, so deleting it completely is a good idea.

`sudo rm /var/www/html/info.php`

### Sources

While I used many sources, many of them contained exactly the same information and/or commands so I'm only listing the ones that contained additional information.

* Mastering CentOS 7 Linux Server by Bhaskarjyoti Roy