---
layout: post
title:  "Load Balancer for EMQ X Cluster"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/posts/blur-computer-connection-442150.jpg
description: "Load Balancer for EMQ X Cluster."
tags: [featured]
---

Load balancing is used to distribute network load among multiple components or software systems. **Load Balancers** support for different load balancing algorithms like *roundrobin*, *random*, *by weight* etc maximizes utilization of *cluster nodes* and reduces the chances of *service unavailability* due to *over loading* of some *member nodes*.

## Benefits of using Load Balancer with EMQ X Cluster
Following is the list of core benefits we get by introducing a *load balancer* into a **EMQ X Cluster**.

### High Availability
High Availability of EMQ X Broker nodes by avoiding failure of any member node of **EMQ X Cluster**.

### Improved responsiveness
Improved responsiveness of **EMQ X Cluster** services by reducing the response time.

### Optimized Resource Usage
Optimized usage of available resources by forwarding the network traffic to  most optimal / available broker nodes of **EMQ X Cluster**. 

Most of the *Load Balancers* support *SSL/TLS*, this can be used to terminate *SSL/TLS* connection at *load balancer* end and plain connections can be forwarded to *EMQ X Broker nodes*. Load on *EMQX Broker nodes* can be reduced by offloading the *SSL/TLS* connections.   

### Simplification of client-side Configurations
Without *load balancer* you will have to put address of all *EMQ X Broker Nodes* for  *EMQ X MQTT Client*. With *load balancer* you just need to specify *load balancer url* which makes configuration of *MQTT Client* simple.

### Improved Security
With *load balancer* in front *EMQ X Broker nodes* are not exposed directly to external world so outside world does not know about the internal network resulting in improved security.

You can configure *load balancer* for **EMQ X Cluster** on *public cloud providers* and *private cloud networks* using *load balancer service* offered by *public cloud providers* or software solutions like *HAProxy* or NGINX.

To keep this article simple and concise, I have divided it into multiple sections. I will explain configuration and setup of all major *load balancer* solutions for *public cloud providers* and *private cloud / networks* in following articles.

**Upcoming articles* in this series.

- EMQ X deployment with HAProxy Load Balancer on private network.
- EMQ X deployment with ELB Load Balancer on Amazon AWS Cloud.
- EMQ X deployment with NGINX Plus Load Balancer on private network.

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

## Other articles in MQTT / EMQ X  series
- [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %})
- [Understanding EMQ X broker cluster concepts]({{ site.baseurl }}{% post_url 2019-03-22-understanding-emq-x-broker-cluster-concepts %})
- [How to manually setup EMQ X cluster?]({{ site.baseurl }}{% post_url 2019-03-23-how-to-manually-setup-emqx-cluster %})
- [Clustering EMQ X Automatically:static, using a static node list]({{ site.baseurl }}{% post_url 2019-03-23-clustering-emqx-automatically-using-a-static-node-list %})
- [EMQ X deployment with HAProxy Load Balancer on private network]({{ site.baseurl }}{% post_url 2019-03-26-emqx-deployment-with-haproxy-load-balancer-on-private-network %})