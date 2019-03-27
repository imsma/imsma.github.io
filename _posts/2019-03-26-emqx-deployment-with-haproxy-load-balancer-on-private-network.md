---
layout: post
title:  "EMQ X deployment with HAProxy Load Balancer on private network"
author: sma
categories: [ MQTT,EMQX,IoT,M2M,HAProxy,LoadBalancer ]
image: assets/images/posts/business-close-up-communication-704767.jpg
description: "How to deploy EMQ X cluster with HAProxy Load Balancer on private network?"
tags: [featured]
---

[HAProxy](http://www.haproxy.org)(**H**igh **A**vailibility **Proxy**) is an *open source* *TCP / HTTP* proxy solution to create *highly available* systems. *HAProxy* is widely used *open source* *load balancer*. In this article we will learn how to use *HAProxy* as *load balancer* for *EMQX Cluster*.

To get yourself familiar with basic concepts of *HAProxy* and it's installation process please refer to following articles.

- [Understanding Core Concepts of HAProxy as Load Balancer Solution]({{ site.baseurl }}{% post_url 2019-03-25-understanding-core-concepts-of-haproxy-as-load-balancer-solution %}){:target="_blank"}.
- [How To Install HAProxy on Ubuntu Linux Server]({{ site.baseurl }}{% post_url 2019-03-25-how-to-install-haproxy-on-ubuntu-linux-server %}){:target="_blank"}.

In my previous article [Clustering EMQ X Automatically:static, using a static node list]({{ site.baseurl }}{% post_url 2019-03-23-clustering-emqx-automatically-using-a-static-node-list %}){:target="_blank"}, I explained how to setup cluster of three EMQX broker nodes *using static node list*. In this article we will add *load balancing* capability to this *cluster* using *HAProxy*. 

Let's get started.

## Configure HAProxy Backend for *EMQX nodes* and *EMQX Dashboard*

We will configure *EMQX nodes* as *backend* of *HAProxy*, this means *requests* received by *HAProxy* will be forwarded to *EMQX nodes* defined as *backend*.

Go to the system with *HAProxy* installed and open *HAProxy configuration* file as following.

```
sudo nano /etc/haproxy/haproxy.cfg 
```

This will load the `haproxy.cfg` configuration file with default contents, go to the end of `haproxy.cfg` file and define *backend* for *EMQX nodes* as following.

```
backend backend_emqx_tcp
    balance roundrobin
    server emqx_node_1 192.168.0.31:1883 check
    server emqx_node_2 192.168.0.32:1883 check
    server emqx_node_3 192.168.0.33:1883 check
```

Next we will define *backend* for *EMQX Dashboard* access.

```
backend backend_emqx_dashboard
    balance roundrobin
    server emqx_node_1 192.168.0.31:18083 check
    server emqx_node_2 192.168.0.32:18083 check
    server emqx_node_3 192.168.0.33:18083 check
```

## Configure HAProxy Frontend for *EMQX nodes* and *EMQX Dashboard*

Now we will define *HAProxy frontends* to forward incoming requests for *EMQX node* and *EMQX Dashboard* to respective *backend* defined in previous section.

Open *HAProxy* configuration file (`/etc/haproxy/haproxy.cfg`) in your favorite text editor, move to the end of file and add configurations for *EMQX nodes frontend* as following. 

```
frontend frontend_emqx_tcp
    bind *:1883
    option tcplog
    mode tcp
    default_backend backend_emqx_tcp
```

Next define the *frontend configuration* for *EMQX Dashboard*.

```
frontend frontend_emqx_dashboard
    bind *:18083
    option tcplog
    mode tcp
    default_backend backend_emqx_dashboard
```

## Restart HAProxy service and Check Status of EMQX Backend and Frontend

Restart the *haproxy* service to reload the configurations changes we made in previous section.

```
sudo service haproxy restart
```

Check current status of *haproxy* service.

```
sudo service haproxy status
```

This will print current status of *haproxy* service.

```
● haproxy.service - HAProxy Load Balancer
   Loaded: loaded (/lib/systemd/system/haproxy.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2019-03-25 21:40:37 PKT; 3h 8min ago
     Docs: man:haproxy(1)
           file:/usr/share/doc/haproxy/configuration.txt.gz
  Process: 12748 ExecStartPre=/usr/sbin/haproxy -f ${CONFIG} -c -q (code=exited, status=0/SUCCESS)
 Main PID: 12753 (haproxy-systemd)
    Tasks: 3
   Memory: 1.0M
      CPU: 321ms
   CGroup: /system.slice/haproxy.service
           ├─12753 /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
           ├─12754 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
           └─12758 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds

Mar 25 21:40:37 u-srv-1 systemd[1]: Starting HAProxy Load Balancer...
Mar 25 21:40:37 u-srv-1 systemd[1]: Started HAProxy Load Balancer.
Mar 25 21:40:37 u-srv-1 haproxy-systemd-wrapper[12753]: haproxy-systemd-wrapper: executing /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
u1@u-srv-1:~$ sudo service haproxy restart
u1@u-srv-1:~$ sudo service haproxy status
● haproxy.service - HAProxy Load Balancer
   Loaded: loaded (/lib/systemd/system/haproxy.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2019-03-26 00:49:33 PKT; 2s ago
     Docs: man:haproxy(1)
           file:/usr/share/doc/haproxy/configuration.txt.gz
  Process: 12858 ExecStartPre=/usr/sbin/haproxy -f ${CONFIG} -c -q (code=exited, status=0/SUCCESS)
 Main PID: 12863 (haproxy-systemd)
    Tasks: 3
   Memory: 1.5M
      CPU: 18ms
   CGroup: /system.slice/haproxy.service
           ├─12863 /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
           ├─12865 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
           └─12868 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds

Mar 26 00:49:33 u-srv-1 systemd[1]: Started HAProxy Load Balancer.
Mar 26 00:49:33 u-srv-1 haproxy-systemd-wrapper[12863]: haproxy-systemd-wrapper: executing /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy backend_emqx_tcp started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy backend_emqx_tcp started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy backend_emqx_dashboard started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy backend_emqx_dashboard started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy frontend_emqx_tcp started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy frontend_emqx_tcp started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy frontend_emqx_dashboard started.
Mar 26 00:49:33 u-srv-1 haproxy[12865]: Proxy frontend_emqx_dashboard started.
```
The *last 8 lines* of output of *haproxy service status* confirms the successful start of our *backend* and *frontend* services.

## Test the EMQX Cluster with HAProxy Load Balancer

Let's connect to *EMQX Dashboard* using the *IP or Hostname* of *HAProxy*.

Launch a web browser visit http://HAPROXY_HOSTNAME_OR_IP:18083, or use *mosquitto mqtt client* to connect to *EMQX Cluster* using *HAProxy* node.

```
mosquitto_pub -h HAPROXY_HOSTNAME_OR_IP -p 1883 -t myTopic -m HelloMQTT
```

**Note:** Replace *HAPROXY_HOSTNAME_OR_IP* with valid *host name* or *ip address* assigned to system with *HAProxy*.

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

## Other articles in MQTT / EMQ X  series
- [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %})
- [Understanding EMQ X broker cluster concepts]({{ site.baseurl }}{% post_url 2019-03-22-understanding-emq-x-broker-cluster-concepts %})
- [How to manually setup EMQ X cluster?]({{ site.baseurl }}{% post_url 2019-03-23-how-to-manually-setup-emqx-cluster %})
- [Clustering EMQ X Automatically:static, using a static node list]({{ site.baseurl }}{% post_url 2019-03-23-clustering-emqx-automatically-using-a-static-node-list %})
- [Load Balancer for EMQ X Cluster]({{ site.baseurl }}{% post_url 2019-03-24-load-balancer-for-emq-x-cluster %})
