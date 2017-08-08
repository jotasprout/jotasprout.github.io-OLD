---
layout: post
title:  "F is for FTP"
date:   2017-08-08 10:00:00 -0500
categories: server admin
---
Instructions for setting up an FTP server on my CentOS7 VPS compiled and adapted from various sources and tested by yours truly. 

My new host highly recommended using SFTP (specifically, in FileZilla's Site Manager). All but one of the tutorials I'm reviewing use and recommend VSFTP (so I'm just disregarding the fifth one).

Update Your Yumminess

`yum check-update`

I haven't seen that one before. I'll look it up and explain the difference.

Install Very Secure FTP Daemon. Can I just say that the word "daemon" has always seemed magical and mysterious to me? Like whenever you get a "forbidden" warning?

`sudo yum -y install vsftpd`

Start it and tell it to start upon boot

    sudo systemctl start vsftpd
    sudo systemctl enable vsftpd

Allow external access to FTP services by opening port 21 "where the FTP daemons are listening" (gives you the creeps and chills, doesn't it?)

    sudo firewall-cmd --permanent --zone=public --add-port=21/tcp
    sudo firewall-cmd --permanent --zone=public --add-service=ftp
    sudo firewall-cmd --reload

Why is the service "ftp" instead of "vsftpd"? Because ftp is the thing we're doing, vsftpd is the tool we're using to do the thing.

Now to configure that tool for security and whatnot. These next steps must be done as root. Make a copy of the config file.

`cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.orig`

`vi /etc/vsftpd/vsftpd.conf`

These are our desired configurations:

	anonymous_enable=NO         # disable  anonymous login
	local_enable=YES			# permit local logins
	write_enable=YES			# enable FTP commands which change the filesystem
	local_umask=022		        # value of umask for file creation for local users
	dirmessage_enable=YES	    # enable showing of messages when users first enter a new directory
	xferlog_enable=YES			# a log file will be maintained detailing uploads and downloads
	connect_from_port_20=YES    # use port 20 (ftp-data) on the server machine for PORT style connections
	xferlog_std_format=YES      # keep standard log file format
	listen_ipv6=YES		        # vsftpd will listen on an IPv6 socket instead of an IPv4 one
	pam_service_name=vsftpd     # name of the PAM service vsftpd will use
	userlist_enable=YES  	    # enable vsftpd to load a list of usernames
	tcp_wrappers=YES  			# turn on tcp wrappers

The above has

	listen=NO   				# prevent vsftpd from running in standalone mode

But the other tutorial that mentions this option has it set to `YES`. 