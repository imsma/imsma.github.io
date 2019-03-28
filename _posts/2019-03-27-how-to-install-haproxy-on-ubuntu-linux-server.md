---
layout: post
title:  "How To Install HAProxy on Ubuntu Linux Server"
author: sma
categories: [ HAProxy, Linux, LoadBalancer, ReverseProxy ]
image: assets/images/posts/access-blurred-background-cable-1629299.jpg
description: "How To Install HAProxy on Ubuntu Linux Server."
tags: [featured]
---

[HAProxy](http://www.haproxy.org) is an *open source* *HTTP / TCP*  proxy solution to create *highly available* systems. *HAProxy* is widely used *open source* *load balancer*, 

In this article I will explain the basic setup of *HAProxy 1.8.x* on *Ubuntu Linux* system.

## Installing HAProxy on Ubuntu Linux Server

Please refer to [Uninstalling HAProxy](#uninstalling-haproxy) section, if you want to uninstall any existing installation of *haproxy* package.

1. Add `ppa:vbernat/haproxy-1.8` *apt repository* to system's software source.
	```
	sudo add-apt-repository ppa:vbernat/haproxy-1.8
	```
2. Update the *apt* repository.
	```
	sudo apt-get update
	```
3. Verify if *HAProxy version 1.8.X* has been added to available list of installable softwares.
	```
	apt-cache policy haproxy
	```
	**Output**
	```
	haproxy:
	Installed: (none)
	Candidate: 1.8.19-1ppa1~xenial
	Version table:
		1.8.19-1ppa1~xenial 500
			500 http://ppa.launchpad.net/vbernat/haproxy-1.8/ubuntu xenial/main amd64 Packages
		1.6.3-1ubuntu0.2 500
			500 http://us.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages
			500 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages
			100 /var/lib/dpkg/status
		1.6.3-1 500
			500 http://us.archive.ubuntu.com/ubuntu xenial/main amd64 Packages	
	```
4. Install *HAProxy* using *apt* package manager. 	
	```bash
	sudo apt-get install haproxy
	```
3. Run the `apt-cache policy` command again to verify installation of *HAProxy version 1.8.X*.
	```
	apt-cache policy haproxy
	```
	The line `Installed: 1.8.19-1ppa1~xenial` from following output, confirms 
	```
	haproxy:
	Installed: 1.8.19-1ppa1~xenial
	Candidate: 1.8.19-1ppa1~xenial
	Version table:
	*** 1.8.19-1ppa1~xenial 500
			500 http://ppa.launchpad.net/vbernat/haproxy-1.8/ubuntu xenial/main amd64 Packages
			100 /var/lib/dpkg/status
		1.6.3-1ubuntu0.2 500
			500 http://us.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages
			500 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages
		1.6.3-1 500
			500 http://us.archive.ubuntu.com/ubuntu xenial/main amd64 Packages	
	```

## Staring and Stopping the HAProxy service

We can start *HAProxy* service using `service` command.

```
sudo service haproxy start
```

Run following command to stop *HAProxy* service.

```
sudo service haproxy stop
```

## Check current status of HAProxy service

We can check current status of *HAProxy* service using `status` option of `service` command.

```
sudo service haproxy status
```

This will **output** current status of *HAProxy* service.

```
● haproxy.service - HAProxy Load Balancer
   Loaded: loaded (/lib/systemd/system/haproxy.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2019-03-25 21:40:37 PKT; 2s ago
     Docs: man:haproxy(1)
           file:/usr/share/doc/haproxy/configuration.txt.gz
  Process: 12748 ExecStartPre=/usr/sbin/haproxy -f ${CONFIG} -c -q (code=exited, status=0/SUCCESS)
 Main PID: 12753 (haproxy-systemd)
    Tasks: 3
   Memory: 1.0M
      CPU: 13ms
   CGroup: /system.slice/haproxy.service
           ├─12753 /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
           ├─12754 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
           └─12758 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds

Mar 25 21:40:37 sb-ha-1 systemd[1]: Starting HAProxy Load Balancer...
Mar 25 21:40:37 sb-ha-1 systemd[1]: Started HAProxy Load Balancer.
Mar 25 21:40:37 sb-ha-1 haproxy-systemd-wrapper[12753]: haproxy-systemd-wrapper: executing /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
```

## Basic Configuration of HAProxy

Basic *configuration* of *HAProxy* is located at `/etc/haproxy/haproxy.cfg`, we can view the contents of default *configuration* of *HAProxy* using `cat` command

```
cat /etc/haproxy/haproxy.cfg
```

This will show the contents of `haproxy.cfg` file as following.

```
global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
	stats timeout 30s
	user haproxy
	group haproxy
	daemon

	# Default SSL material locations
	ca-base /etc/ssl/certs
	crt-base /etc/ssl/private

	# Default ciphers to use on SSL-enabled listening sockets.
	# For more information, see ciphers(1SSL). This list is from:
	#  https://hynek.me/articles/hardening-your-web-servers-ssl-ciphers/
	# An alternative list with additional directives can be obtained from
	#  https://mozilla.github.io/server-side-tls/ssl-config-generator/?server=haproxy
	ssl-default-bind-ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS
	ssl-default-bind-options no-sslv3

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http
```

## Uninstalling HAProxy

You can use following approaches to remove or uninstall *haproxy* from your Ubuntu system.

1. Run following command to remove just *haproxy* package from ubuntu.
	```
	sudo apt-get remove haproxy
	```
2. To remove the *haproxy* package along with dependent packages.
	```
	sudo apt-get remove --auto-remove haproxy
	```
3. Ue the *purge* command, if you also want to delete *configuration* files.
	```
	sudo apt-get purge haproxy
	```
4. You can delete *configuration* along with data files for *haproxy* as following.
	```
	sudo apt-get purge --auto-remove haproxy
	```

**Related Articles**

- [Understanding Core Concepts of HAProxy as Load Balancer Solution]({{ site.baseurl }}{% post_url 2019-03-25-understanding-core-concepts-of-haproxy-as-load-balancer-solution %}){:target="_blank"}.


Thanks for reading. I hope you enjoyed this article. If you like this article, have any questions or suggestions please let us know in the comments section.

Happy Learning!
