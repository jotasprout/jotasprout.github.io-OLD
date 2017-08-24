---
layout: post
title:  "A is for Apache"
date:   2017-08-03 10:00:00 -0500
categories: server admin
---
This post will include an explation of `<VirtualHost *:80>` and how that differs from `ServerName`. The whole post will explain reconciling the domain name with the IP address.

Instructions adapted from my two library books, 

Update Your Yumminess

`sudo yum -y update`

What's the "-y" for?

The "y" tells the terminal to answer "yes" to any prompts. "Yes to all."

Install Apache

`yum install httpd`

The tute I used said

`sudo yum -y install httpd`

What's the "-y" for?

Start apache and tell it to start upon boot

    sudo systemctl start httpd
    sudo systemctl enable httpd

Do I also have to do the following? What's the difference? DigitalOcean is one tutorial that has these instead.

    systemctl start httpd.service
    systemctl enable httpd.service

Let's test it

`sudo systemctl status httpd`

While setting up our server, we installed and enabled a firewall. Now, we need to make sure it allows HTTP and HTTPS traffic.

    sudo firewall-cmd --permanent --zone=public --add-service=http
    sudo firewall-cmd --permanent --zone=public --add-service=https
    sudo firewall-cmd --reload

Also this? Does doing either above or below automatically do the other?

    sudo firewall-cmd --permanent --add-port=80/tcp
    sudo firewall-cmd --permanent --add-port=443/tcp
    sudo firewall-cmd --reload

Directing your browser to your server's IP address should open the Apache placeholder page. If it does, you rock--Apache is up and running.

But I don't understand where that page is coming from. There's nothing in the `/var/www/html` folder (where, I'm told, I should put my site files). Even once I put a file in there and comment out the stuff in `/etc/httpd/conf.d/welcome.conf`, the default Apache page appears. 

I found out where it's coming from and how to tell it not to do that. FIND THOSE STEPS AND PUT THEM HERE.

Your little Apache server can host multiple web sites with different domains using virtual hosts (think about how your host divides its servers into "virtual private servers"). So, now, we'll create a virtual host for your website.

First, let's create the directory in which our first (if not only) website will live.

`mkdir /var/www/example.com/public_html`

The books use `home` instead of `html` or `public_html`. I don't know why. Apparently, `htdocs` is an equivalent alternative to `public_html`.

Change ownership of the folder from you to your admin user (not root)

`chown -R username:username /var/www/example.com/public_html`

Why?

Allow visitors to see/read the `www` directory by changing the permissions

`chmod 755 /var/www`

Tell Apache to Look for Virtual Hosts

Some tutes (and my two CentOS library books) say to use Nano. Based on what I've read, I choose to use Vi. Apparently, the learning curve for Vi is steeper but it's much more powerful. Myself, I made a trusty cheat-sheet and the curve hasn't been bad. Except for that time I accidentally deleted a whole line of code and broke something. 

For that reason, you might want to make these changes to a copy or make a backup copy.

`cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.backup`

Open Apache's main configuration file

I think you have to do this as root ... so, type

`su - root`

then

`sudo vi /etc/httpd/conf/httpd.conf`

Find lines that look like the following and make sure they are listening to port 80 and (at least?) the second line is not commented out (remove the "#" from the beginning of the line).

Find the following line and uncomment it

`#NameVirtualHost *:80`

The Servermom.org tute, "How to Add New Site Into Your Apache-based CentOS Server" states that below the above line, you'll find something similar to the following and should edit it accordingly. Mine didn't have anything like the following but did have a couple lines of it elsewhere in the doc. I like the idea of having all of this together. The CentOS library books and the DigitalOcean tute said to create our own conf files and add the virtual host in there (along with other instructions at the end).

So, in that file (I guess) or in your own (see bottom) add

    <VirtualHost *:80>
    </VirtualHost>

Between those tags, declare the main server name and an alias so that both point to the same place. Some of the following lines are from serverMom.org

    ServerAdmin webmaster@example.com
    DocumentRoot /var/www/example.com/public_html
    ServerName www.example.com
    ServerAlias example.com
    ErrorLog /var/www/example.com/error.log
    CustomLog /var/www/example.com/requests.log

In some tutes, that last line is shown with `common` at the end (I'm pretty sure it's shown that way as the default and it's always removed by the end). In the library books -- which seem to be different than anything else (because they're British or something? Is that a thing?) -- that last line ends with `combined`.

Let's test beginning with a reboot of Apache

`apachectl -k stop`

That kills all running Apache processes. Then

`service httpd start`

Then, according to serverMom, an index file in the `public_html` directory should work when you put your domain in your browser.

FROM YE OLDE LIBARY BOOKS AND DIGITALOCEAN

Add this to the end of the file (it's for this next thing we'll do--Virtual Host stuff--we need to tell Apache to look for virtual hosts)

`IncludeOptional sites-enabled/*.conf`

Then save and close the file.

Create Virtual Host Files

    sudo mkdir /etc/httpd/sites-available
    sudo mkdir /etc/httpd/sites-enabled

According to DigitalOcean, `sites-available` is for all your files and `sites-enabled` contains symbolic links to the hosts you want published.

Create the First Virtual Host File

Create & open it by

`sudo vi /etc/httpd/sites-available/example.com.conf`

Inside it, do this:

    <VirtualHost *:80>
    </VirtualHost>

Those tags say that their contents is a virtual host listening on port 80. Inside those tags, do this:

    ServerName www.example.com
    ServerAlias example.com

From DigitalOcean: "In order for the www version of the domain to work correctly, the domain's DNS configuration will need an A record or CNAME that points www requests to the server's IP. A wildcard (*) record will also work. To learn more about DNS records, check out our host name setup guide."

Point to the publicly accessible files of/for our site live:

    DocumentRoot /var/www/example.com/public_html

And, lastly, where Apache should store records of errors and requests. 

    ErrorLog /var/www/example.com/error.log
    CustomLog /var/www/example.com/requests.log combined

That's where you check to see how many filthy crackers, thieves and terrorists are trying to break into your site.

DigitalOcean says, "enable them so that Apache knows to serve them to visitors. To do this, we can create a symbolic link for each virtual host in the sites-enabled directory:"

`sudo ln -s /etc/httpd/sites-available/example.com.conf /etc/httpd/sites-enabled/example.com.conf`

Restart because that's what you do.

`sudo apachectl restart`

Now test it in your browser.

Still need to add this material, compare, and consolidate all.

http://www.servermom.org/how-to-add-new-site-into-your-apache-based-centos-server/454/

SOMEWHERE, I HAD TO TELL IT WHAT THE DEFAULT PAGE IN A DIRECTORY IS ... I PUT HTM NOT JUST HTML AND I THINK I NEED TO PUT "index.php" IN THERE AS WELL.

# SECURITY

Mastering CentOS Linux Server and, to a much lesser extent, CentOS Cookbook have GREAT security stuff I need to do.

Something that jumps out at me in Mastering is it says to deactivate Firewalld and install and activate iptables. That might make me happy since Firewalld has been giving me crap. To be fair and in full disclosure, I think it's troubles are due to some incompetence on the part of my host's "support."

### Sources

While I used many sources, many of them contained exactly the same information and/or commands so I'm only listing the ones that contained additional information.

* Mastering CentOS 7 Linux Server by 