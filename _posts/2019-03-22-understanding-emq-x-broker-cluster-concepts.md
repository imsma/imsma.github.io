---
layout: post
title:  "Understanding EMQ X broker cluster concepts"
author: sma
categories: [ MQTT,EMQ,EMQX,IoT,M2M ]
image: assets/images/abstract-ai-art-373543.jpg
description: "Understanding EMQ X broker cluster concepts"
tags: [featured]
---

[EMQX](https://www.emqx.io/) a distributed, highly available and highly scalable message broker. **EMQX** is designed and built using [Erlang/OTP](https://github.com/erlang/otp). In this article I will explain how to setup *EMQ Cluster* manually. 

## Understanding Erlang and OTP

[**Erlang**](https://learnyousomeerlang.com/content) is a functional programming language and runtime to build massively scalable and high available systems.

[**OTP**](http://erlang.org/doc/system_architecture_intro/sys_arch_intro.html) (*Open Telecom Platform*) is a collection of useful **Erlang** tools, libraries and design principals to build softwares using **Erlang** programming language.

## Understanding Erlang/OTP Distributed Runtime and Erlang Runtime Node

**Erlang/OTP Distributed system** is made of multiple **Erlang** runtime systems known as *node*. These *Erlang runtime nodes* communicate with each other by passing *messages* to each other using *TCP/IP* sockets.

**Erlang runtime system / node** is idenitified with a email like name e.g. `node_name@host` or `node@127.0.0.1`. **epmd**(Erlang port mapper daemon) maps the *node name* with *TCP Sockets*. Authentication among *Erlang nodes* happens using *magic cookie*. Following snippet shows how you can set *Erlang node cookie*. Please note that the word `COOKIE` in following snippet is just a placeholder, you need to replace it with actual contents for *Erlang node cookie*.

```
erl -setcookie COOKIE
```

## EMQ X Broker Cluster Architecture and Design

The architecture of **EMQ X Broker Cluster** is based on **Erlang/OTP** and [**Mnesia**](http://erlang.org/doc/man/mnesia.html) database.

### EMQ X Broker Cluster Design Rules
1. *MQTT Client** *SUBSCRIBE* a *TOPIC* on a **EMQ X Broker Cluster Node**, this **node** will inform all other **nodes** in current **Cluster**.
2. *MQTT Client** *PUBLISH* a *MESSAGE* to a **EMQ X Broker Cluster Node**, this node will forward this *MESSAGE* to all the nodes present in **Topic Table**.


### Global Route table of TOPIC -> NODE
A *global route table* with **topic** and **nodes** mappings is replicated to all **nodes** in *cluster*. Following a sample illustration of *global route table*.

{:class="table table-responsive "}
|Topic Name | Nodes                     |
|-----------|---------------------------|
|Topic1     | node1, node2, node3,node4 |
|Topic2     | node1, node3              |
|Topic3     | node2                     |
|Topic4     | node2, node3, node4       |



That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

## Links of other articles in this series
1. [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %})
2. [How to manually setup EMQ X cluster?]({{ site.baseurl }}{% post_url 2019-03-23-how-to-manually-setup-emqx-cluster %})
