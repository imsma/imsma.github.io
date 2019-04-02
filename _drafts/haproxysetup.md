---
layout: post
title:  "How to setup High Availability Cluster of HAProxy using Keepalived"
author: sma
categories: [ HAProxy,Keepalived,HighAvailability ]
image: assets/images/posts/art-board-game-challenge-163064.jpg
description: "How to setup High Availability Cluster of HAProxy using Keepalived?"
tags: [featured]
---

How to setup High Availability Cluster of HAProxy using Keepalived?

HAProxy (High Availability Proxy) serves it's job well as a *Reverse Proxy* and *TCP / HTTP load balancer*.


![HAProxy Load Balancer]({{ site.baseurl }}/assets/images/posts/ha-proxy-load-balancer.jpg)

In *HAProxy load balancing setup* shown in above diagram the *HAProxy* is the single points of failure,which may cause downtime / service unavailability. To make this a true *High Availability setup*, we need to make *HAProxy load balancer* redundant with two or more *HAProxy* nodes using *Keepalived and VRRP*.

![HAProxy Load Balancer]({{ site.baseurl }}/assets/images/posts/ha-proxy-load-balancer-high-availability-setup.jpg)

The example shown in above diagram uses a *virtual ip address* which is currently assigned to *Active HAProxy node* and in case of failure it can be reassigned to the *HAProxy Backup node*. We we explore *How to build redundant with High Availability cluster of multiple HAProxy servers using Keepalived, LVS and VRRP* in following sections of this article.

## Installing HAProxy

Please follow my article [How To Install HAProxy on Ubuntu Linux Server]({{ site.baseurl }}{% post_url 2019-03-27-how-to-install-haproxy-on-ubuntu-linux-server %}) to get *HAProxy* installed on participant servers *(that you want to make redundant)*.

## Install Keepalived

As mentioned above we will create the *High Availability CLuster of HAProxy* using *Keepalived* so you need to install *Keepalived* on all the participant servers with *HAProxy* installed. You can follow my article [Setup Highly Available Private Networking using Keepalived (VRRP)]({{ site.baseurl }}{% post_url 2019-03-27-highly-available-private-networking-using-keepalived %}) to get it done.






That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
