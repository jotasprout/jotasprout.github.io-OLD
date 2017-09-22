---
layout: post
title:  "Ch-is-for-Chown-and-Chmod"
date:   2017-09-22 11:00:00 -0500
categories: linux
---
As with most, if not all, of these posts, this one is inspired by trying to "fix" a problem after moving to a VPS. The problem isn't the VPS and, in fact, isn't really a problem. I just keeping learning I must do a lot of work that cPanel and shared hosting used to take care of for me. Had I known how much I'd learn through practical work, I'd have switched a long, long time ago. 

I've learned--repeatedly--that Permissions are important. Unfortunately, I already had a "P is for" post and, now that I think about it, P isn't for PHP because PHP is for ... dude, what is PHP for ...?

According to [Wikipedia][php-wikipedia], "PHP originally stood for Personal Home Page." [PHP.net][php-net] explains it is now a recursive acronym (an acronym that contains itself) standing for PHP: Hypertext Preprocessor.

What do these commands stand for?

Chown = Change owner of the file, folder, folder and everything in it

Chmod = Changes permissions ... that's all I know at the moment.

Permissions aren't just important for keeping unwanted users out but allowing users in that are needed for stuff to work. For example, part of my main app-in-progress has a file upload feature. I created the destination folder so I was the owner but Apache needs to be the owner. 

Hmm ... who will the owner be for dynamically created sub-folders? I should finish the bit of code that will create them and see ...

[php-wikipedia] https://en.wikipedia.org/wiki/PHP
[php-net] http://php.net/manual/en/faq.general.php