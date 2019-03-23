---
layout: post
title:  "Understanding EMQ X Cluster Configuration"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/abstract-ai-art-373543.jpg
description: "Setup EMQ X broker cluster on Ubuntu Server"
tags: [featured]
---

[EMQX](https://www.emqx.io/) a distributed, highly available and highly scalable message broker. **EMQX** is designed and built using [Erlang/OTP](https://github.com/erlang/otp), and supports all major **IoT** protocols to develop scalable solutions for **M2M** and **mobile application** messaging.

In previous articles  
... emqx node
... emqx cluster
we explored  how to setup basic  node and cluster.

In this article we will deep dive into *EMQ X Cluster Configuration*.

// https://developer.emqx.io/docs/emq/v3/en/config.html#emq-x-cluster

## Cluster Name

```
## Cluster name
cluster.name = emqcl
```

## Cluster Discovery Mechanism

```
## Cluster discovery strategy: manual | static | mcast | dns | etcd | k8s
cluster.discovery = manual
```

## Cluster Autoheal

```
## Cluster Autoheal: on | off
cluster.autoheal = on
```

## Cluster Autoclean

```
## Clean down node of the cluster
cluster.autoclean = 5m
```

## Auto-discovery Strategy for EMQ X Broker

EMQ X 3.0 supports node discovery and autocluster with various strategies:

Strategy	Description
manual	Create cluster manually
static	Autocluster by static node list
mcast	Autocluster by UDP Multicast
dns	Autocluster by DNS A Record
etcd	Autocluster using etcd
k8s	Autocluster on Kubernetes



## Manual Creating EMQ X broker cluster 

This is the default configuration of clustering, nodes joins a cluster ./bin/emqx_ctl join <Node> command:

```
cluster.discovery = manual
```

## Autocluster by static node list¶

```
cluster.discovery = static

##--------------------------------------------------------------------
## Cluster with static node list

cluster.static.seeds = emqx1@127.0.0.1,ekka2@127.0.0.1
```

## Autocluster by IP Multicast

```
cluster.discovery = mcast

##--------------------------------------------------------------------
## Cluster with multicast

cluster.mcast.addr = 239.192.0.1

cluster.mcast.ports = 4369,4370

cluster.mcast.iface = 0.0.0.0

cluster.mcast.ttl = 255

cluster.mcast.loop = on
```

## Autocluster by DNS A Record

```
cluster.discovery = dns

##--------------------------------------------------------------------
## Cluster with DNS

cluster.dns.name = localhost

cluster.dns.app  = ekka
```

## Autocluster using etcd

```
cluster.discovery = etcd

##--------------------------------------------------------------------
## Cluster with Etcd

cluster.etcd.server = http://127.0.0.1:2379

cluster.etcd.prefix = emqcl

cluster.etcd.node_ttl = 1m
```

## Autocluster on Kubernetes

```
cluster.discovery = k8s

##--------------------------------------------------------------------
## Cluster with k8s

cluster.k8s.apiserver = http://10.110.111.204:8080

cluster.k8s.service_name = ekka

## Address Type: ip | dns
cluster.k8s.address_type = ip

## The Erlang application name
cluster.k8s.app_name = ekka
```

## EMQ X Node and Cookie

The node name and cookie of EMQ X should be configured when clustering:

```
## Node name
node.name = emqx@127.0.0.1

## Cookie for distributed node
node.cookie = emqx_dist_cookie
```

## 

```

```


## 

```

```

## 

```

```


## 

```

```

## 

```

```

## 

```

```

## 

```

```

## 

```

```

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
