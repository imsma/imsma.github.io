---
layout: post
title:  "Setup EMQX broker node on Ubuntu Server"
author: sma
categories: [ MQTT,emq,IoT,M2M ]
image: assets/images/city-downtown-light.jpg
description: "Setup EMQX broker node on Ubuntu Server"
---

[EMQX](https://www.emqx.io/) a distributed, highly available and highly scalable message broker. **EMQX** is designed and built using [Erlang/OTP](https://github.com/erlang/otp), and supports all major **IoT** protocols to develop scalable solutions for **M2M** and **mobile application** messaging.

Following steps will help you setup **EMQX** broker on **Ubuntu Server**.

#### Install erlang [View detailed article]({{ site.baseurl }}{% post_url 2019-03-10-install-erlang %})

```
$ apt-get install erlang
```

#### Download the deb package

```
$ wget https://www.emqx.io/downloads/broker/v3.0.0/emqx-ubuntu16.04-v3.0.0_amd64.deb
```


#### Install the EMQX deb package

```
$ sudo dpkg -i emqx-ubuntu12.04-v3.0_amd64.deb
```


#### Erlang/OTP R19 depends on lksctp-tools library

```
$ apt-get install lksctp-tools
```