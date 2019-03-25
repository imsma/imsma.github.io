---
layout: post
title:  "Understanding Core Concepts of HAProxy as Load Balancer Solution"
author: sma
categories: [ HAProxy,LoadBalancer, DistributedSystems ]
image: assets/images/posts/boat-boat-rope-fishing-boat-1050656.jpg
description: "Understanding HAProxy core concepts including HAProxy backend, HAProxy frontend, HAProxy ACL and pattern matching, HAProxy load balancing algorithms."
tags: [featured]
---

[HAProxy](http://www.haproxy.org)(**H**igh **A**vailibility **Proxy**) is an *open source* *TCP / HTTP* proxy solution to create *highly available* systems. *HAProxy* is widely used *open source* *load balancer*.

In following sections we will look at some important concepts of *HAProxy*.

## HAProxy ACL (Access Control List)

*HAProxy ACL* allows us to define *rules* like *pattern matching* to *forward* or *block* some traffic to specific servers.

**Example**

```
acl url_product path_beg /product
```
Above *ACL* will match any urls beginning with */product* for example `http://www.example.com/product/product1` and `http://www.example.com/product/product2`. Please refer to *HAProxy website* for detailed documentation  of [Using ACLs and pattern extraction](http://cbonte.github.io/haproxy-dconv/configuration-1.4.html#7){:target="_blank"}.

## Understating HAProxy Backend
In context of *HAProxy*, *backend* is a list of *servers* that will ultimately receive requests forwarded by *HAProxy*. Configuration of *HAProxy backend* is done in *backend* section of *HAProxy configuration* file located at `/etc/haproxy/haproxy.cfg`.

Following is a sample configuration for *HAProxy backend* named *http-backend*.

```
backend http-backend
   balance roundrobin
   server srv1 srv1.example.com:80 check
   server srv2 srv2.example.com:80 check
```

**Where:**
- **backend http-backend**  : declares the name of *backend* as *http-backend*.
- **balance roundrobin**    : Specifies *roundrobin* as the *load balancing algorithm*.
- **server srv1 srv1.example.com:80 check**  : declares *srv1.example.com:80* as first backend server to receive the requests forwarded to this *http-backend*.
- **server srv1 srv1.example.com:80 check**  : declares *srv2.example.com:80* as second backend server to receive the requests forwarded to this *http-backend*.
- **check** : *check* option tells the *HAProxy* to do *health check* on backend server.

## Understating HAProxy Frontend
How the incoming requests will be forwarded to *backends* is defined using *frontend* configuration section of *HAProxy*. 

Following is a sample configuration for *HAProxy frontend* named *http-frontend*.

```
frontend http-frontend
    bind *:8080
    mode http
    default_backend http-backend
```

**Where:**
- **frontend http-frontend**  : declares the name of *frontend* as *http-frontend*.
- **bind \*:8080**  : *http-frontend* will handle network traffic received port *8080* of any (*) IP assigned to machine where *HAPoxy* is installed.
- **mode http**  : *http-frontend* will handle only *HTTP* traffic.
- **default_backend http-backend** : defines the default backend as *http-backend** for *http-frontend*.

## Understating HAProxy Load Balancing Algorithms

*HAProxy* uses **Load Balancing Algorithm** to select a *backend server* for *load balancing*. The core *load balancing algorithms* supported by *HAProxy* are *roundrobin*, *static-rr*, *leastconn*, *source*, *uri*, *url_param*, *hdr* and *rdp-cookie*. In following sections I will explain concepts and working of some core *load balancing algorithms for HAProxy*, for detailed documentation please refer to [algorithm](http://cbonte.github.io/haproxy-dconv/configuration-1.4.html#4.2-balance){:target="_blank"}. section of *HAProxy's* official documentation.

### roundrobin algorithm for load balancing

**roundrobin** load balancing algorithm specifies that each *backend server* will be used in turn as per their weight. The weight of a server is determined dynamically. According to *HAProxy* documentation, *roundrobin* algorithm has a limitation of *4095* servers per *backend*.

### static-rr algorithm for load balancing

Like *roundrobin* algorithm, **static-rr** load balancing algorithm also uses *backend server*  in turns. As the name specifies this is a *static* algorithm, which means *server weight* will not be determined on the fly. **static-rr** algorithm does not impose any limit of number of servers by *backend*.

### leastconn algorithm for load balancing

When *load balancing* using **leastconn** algorithm, server with *least* connections will receive the next incoming *request*.


### source algorithm for load balancing

The source IP address is hashed and divided by the total
              weight of the running servers to designate which server will
              receive the request.

Using **source** *load balancing algorithm*, *backend server* is selected on the basis of a *hash of user's IP address* to make sure that *user* connect to same server.


**Related Articles**
[How To Install HAProxy on Ubuntu Linux Server]({{ site.baseurl }}{% post_url 2019-03-25-how-to-install-haproxy-on-ubuntu-linux-server %}).

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
