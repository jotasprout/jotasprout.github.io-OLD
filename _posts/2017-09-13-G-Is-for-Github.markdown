---
layout: post
title:  "G is for Github"
date:   2017-09-13 9:00:00 -0500
categories: development
---
One of the many cool things that happened during Hurricane Irma was a chance encounter in a hotel lobby.

## Github Desktop

[Download this][downwarddesk] insanely cool and easy to use desktop app available for Windows, MacOS, and Linux.

Write a couple cool things.

[downwarddesk]: https://help.github.com/desktop/guides/getting-started/installing-github-desktop/#platform-mac

## Force Pulling

I am the only team member on most of my projects but I do work on two computers. I try my best to be vigilant about always pushing when I'm done with a given machine but, sometimes, when I sit at my laptop or desktop and `git pull` the process is aborted because there's a change or two locally that would get overridden. More than likely, I want to override it because I know the last time I worked (on the other machine), I did significant work I need to continue. Here are the steps for forcing your git to pull from your remote repo.

Explain each ...

	git fetch --all
	git reset --hard origin/master
	git pull origin master


