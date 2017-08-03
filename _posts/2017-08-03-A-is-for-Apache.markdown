---
layout: post
title:  "A is for Apache"
date:   2017-08-03 10:00:00 -0500
categories: server admin
---
This post will include an explation of `<VirtualHost *:80>` and how that differs from `ServerName`. The whole post will explain reconciling the domain name with the IP address.

Install Apache
`yum install httpd`

While setting up our server, we installed and enabled a firewall. Now, we need to make sure it allows HTTP and HTTPS traffic.

`sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload`

Create Virtual Host Files
`sudo mkdir /etc/httpd/sites-available
sudo mkdir /etc/httpd/sites-enabled`

Tell Apache to Look for Virtual Hosts
Open Apache's main configuration file
`sudo nano /etc/httpd/conf/httpd.conf`

Add this to the end of the file
`IncludeOptional sites-enabled/*.conf`
Then save and close the file.

Create the First Virtual Host File
Create & open it by
`sudo nano /etc/httpd/sites-available/example.com.conf`

Add
`<VirtualHost *:80>
</VirtualHost>`

Between those tags we declare the main server name and an alias so that both point to the same place.
`ServerName www.example.com
ServerAlias example.com`