---
layout: post
title:  "F is for FTP"
date:   2017-08-08 10:00:00 -0500
categories: server admin
---
Instructions for setting up an FTP server on my CentOS7 VPS compiled and adapted from various sources and tested by yours truly. 

My new host highly recommended using SFTP (specifically, in FileZilla's Site Manager). All but one of the tutorials I'm reviewing use and recommend VSFTP (so I'm just disregarding the fifth one).

# Update Your Yumminess

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

These configurations come from Tecmint and are similar to tutes except where indicated:

	anonymous_enable=NO        		 	# disable  anonymous login
	local_enable=YES					# permit local logins
	write_enable=YES					# enable FTP commands which change the filesystem
	local_umask=022		        		# value of umask for file creation for local users
	dirmessage_enable=YES	    		# enable showing of messages when users first enter a new directory
	xferlog_enable=YES					# a log file will be maintained detailing uploads and downloads
	connect_from_port_20=YES    		# use port 20 (ftp-data) on the server machine for PORT style connections
	xferlog_std_format=YES      		# keep standard log file format
	listen_ipv6=YES		        		# vsftpd will listen on an IPv6 socket instead of an IPv4 one
	pam_service_name=vsftpd     		# name of the PAM service vsftpd will use
	userlist_enable=YES  	    		# enable vsftpd to load a list of usernames
	userlist_file=/etc/vsftpd.userlist  # stores usernames
	userlist_deny=NO					# the list doesn't deny, but grants access
	tcp_wrappers=YES  					# turn on tcp wrappers

The above has

	listen=NO   				# prevent vsftpd from running in standalone mode

But the other tutorial that mentions this option has it set to `YES`. 

Enable passive mode. Why?

	pasv_enable=Yes
	pasv_min_port=40000
	pasv_max_port=40100

Still wondering about difference between "regular" and "service"

	systemctl restart vsftpd.service
	systemctl enable vsftpd.service

<h2>ALLOW AND DENY USERS</h2>

You can grant or deny FTP access by listing users in `/etc/vsftpd.userlist`. First, enable the user list by setting `userlist_enable=YES` (above). If you want users listed to be denied access, set `userlist_deny=YES` (above). Setting `userlist_deny=NO` means that only users listed in the file are allowed/granted FTP login access. 

<h3>RESTRICTING USERS</h3>

	chroot_local_user=YES
	allow_writeable_chroot=YES

`chroot_local_user=YES` restricts the user to their home directory.

`allow_writeable_chroot=YES` allows them to write to that directory. Be aware of the security implications of users having upload permissions and shell access. 

Tecmint step 5 gives some great, more secure, steps to take regarding alternate "home" directories.

The Krizna tute says "Setup SELinux to allow ftp access to the users home directories." How is that different from above? 

`setsebool -P ftp_home_dir on`

**Tecmint**, however, cites a RedHat bug report as the reason for disabling the `ftp_home_dir` directive by default and, instead, "use `semanage` command to set SELinux rule to allow FTP to read/write user’s home directory."

`semanage boolean -m ftpd_full_access --on`

Once all of this is done, restart vsftpd.

`systemctl restart vsftpd`

Create a FTP user and set the password for that user. DigitalOcean's humble user instructions:

	useradd ftpuser
	passwd ftpuser

**Tecmint**'s tute has some interesting shizzle I need to look up

	useradd -m -c “Ravi Saive, CEO” -s /bin/bash ravi
	passwd ravi

**Tecmint** then adds Ravi to the user list using some shizzle I need to look up.

	echo "ravi" | tee -a /etc/vsftpd.userlist
	cat /etc/vsftpd.userlist

**Krizna** states "`/sbin/nologin` shell is used to prevent shell access to the server."

	useradd -m dave -s /sbin/nologin
	passwd dave

Krizna states, "Now user dave can able to login ftp on port 21 ftp" using FileZilla. The screenshot shows the port field empty, Protocol set to FTP, Encryption set to "Use plain FTP" and Logon Type set to "Account."

Then, right after that, Krizna has a section on installing openssh-server to "enable" SFTP:

>"Secure File Transfer Protocol is used to encrypt connections between clients and the FTP server. It is highly recommended to use SFTP because data is transferred over encrypted connection using SSH-tunnel on port 22."

My new host specifically does not use 22 and says to use SFTP.

Krizna tute goes on to create SFTP users and those instructions are confusing. Lastly, mentions using FTP and SFTP together and concludes with other changes for existing users I need to review.

TUTORIALSPOINT SIDEBAR START

TutorialsPoint states using `–M` instead of `–m` creates a user without a home directory. So `-m` creates a user with a home directory? What's the `-s` for?

	useradd -M user1 –s /sbin/nologin
	passwd user1

TutorialsPoint then creates a home directory (named "mike") for the user (user1) they just created (specifically without a home directory -- what's the purpose behind this?).

	mkdir /var/www/mike
	chmod 755 /var/www/mike

Grant user1 FTP access to the folder named "mike"

`chown -R mike /var/www/user1`

TUTORIALSPOINT SIDEBAR END

Set the webroot as our FTP user's home directory.

`ftpuser:x:1002:1002::/var/www/html/mysite:/bin/bash`

I need to reconcile the above webroot with whatever is in my other post(s).

Add user to apache group created in Apache post.

`usermod -a -G apache ftpuser`

Change permissions to apache group on webroot. What does that mean?

`chgrp -R apache /var/www/html/webroot/`

TESTING WITH TESTS FOR TESTING

* Try logging in anonymously.
* Try logging in as a user listed (or not) in the user list.
* Confirm user's are in their home (or desired alternate) directory.

BUT NOW I'M CONFUSED

DigitalOcean's "How To Configure vsftpd to Use SSL/TLS on a CentOS VPS" states, 

>"Warning: FTP is insecure! Consider using SFTP instead of FTP. [FTP] has fallen out of favor due to the lack of security inherent in its design. A very capable alternative is SFTP ... This protocol implements file sharing over SSH. If you must use FTP, you should at least secure the connection with SSL/TLS certificates."

The first thing the tute does is install vsftpd which I thought was VERY secure, hence the name and, thus, more secure than SFTP which I inferred was "merely" secure. I need to research this.