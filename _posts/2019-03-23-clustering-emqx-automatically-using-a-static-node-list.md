---
layout: post
title:  "Clustering EMQ X Automatically:static, using a static node list"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/cables-close-up-connection-159304.jpg
description: "How to setup EMQ X cluster automatically using a static node list?"
tags: [featured]
---

The simplest way to automatically create **EMQ X Broker cluster** is to use configure list of *static nodes* on each *member node* of **EMQ X Broker cluster**.

Let's start with creating cluster of 3 **EMQ X Broker** *nodes*.

## 1. Setup EMQ X Broker Nodes

Follow the guidelines presented in my previous article [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %}){:target="_blank"} to setup **3 EMQ X nodes**. So we have **3 EMQ X nodes** setup as following.

{:class="table table-responsive "}
| # | Node Name         | IP Address     |
|---|-------------------|----------------|
| 1 | emqx@192.168.0.31 | 192.168.0.31   |
| 2 | emqx@192.168.0.32 | 192.168.0.32   |
| 3 | emqx@192.168.0.33 | 192.168.0.33   |

You can set **node name of EMQ X Broker Node** using the `node.name` property defined in `/etc/emqx/emqx.conf` configuration file. Following are detailed steps to set the **node name of EMQ X Broker Node**, follow these steps to setup `node.name` property on each member node.

## Set EMQ X Cluster Cookie

All *nodes* in **EMQ X Cluster** need to have same *cookie value*, this helps **EMQ X** to recognize *nodes* of the same **EMQ X Cluster**. You can set *cookie value* of **EMQ X Broker Node** using `node.cookie` property in `/etc/emqx/emqx.conf` configuration file.

## Configuring the EMQ X Cluster Mode
We can set the *cluster mode* for **EMQ X Cluster** using `cluster.discovery` property in `/etc/emqx/emqx.conf` configuration file.

1. Open the `/etc/emqx/emqx.conf` configuration file.
```
sudo nano /etc/emqx/emqx.conf
```
2. Search for `cluster.discovery` property.
```
## Value: Enum
## - manual: Manual join command
## - static: Static node list
## - mcast:  IP Multicast
## - dns:    DNS A Record
## - etcd:   etcd
## - k8s:    Kubernates
##
## Default: manual
cluster.discovery = manual
```
3. Set `static` as value of `cluster.discovery` property.
```
cluster.discovery = static
```
Please note that default value for `cluster.discovery` property is `manual`, you need to change value of `cluster.discovery` property to `static` for each *member node* of **EMQ X Cluster**.

## Configure EMQ X Cluster Nodes
To setup **EMQ X Cluster** with *static mode* you need to set *node names* of all *member nodes* against `cluster.static.seeds` property in `/etc/emqx/emqx.conf` configuration file of each *member node*.

Following are the steps to configure each *member node* to create **EMQ X Cluster** with *static mode*.

1. Open `/etc/emqx/emqx.conf` (EMQ X configuration) file for editing.
```
sudo nano /etc/emqx/emqx.conf
```
2. Search for `cluster.static.seeds` property.

```
## Cluster using static node list

## Node list of the cluster.
##
## Value: String
## cluster.static.seeds = emqx1@127.0.0.1,emqx2@127.0.0.1

```

3. Set comma separated list  of *node names* of all *member nodes*.
```
cluster.static.seeds = emqx@192.168.0.31,emqx@192.168.0.32,emqx@192.168.0.33
```

Repeat *step 1* to *step 3* for all *member nodes* of **EMQ X Cluster**.


## Checking Status of EMQ X Cluster
We can use `emqx_ctl cluster status` command to check status of **EMQ X Cluster**. Run `emqx_ctl cluster status` command on any *member node* of **EMQ X Cluster** as following.
```
emqx_ctl cluster status
```
**Output**
```
Cluster status: [{running_nodes,['emqx@192.168.0.31','emqx@192.168.0.32',
                                 'emqx@192.168.0.33']}]
```


That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

## Links of other articles in this series
- [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %})
- [Understanding EMQ X broker cluster concepts]({{ site.baseurl }}{% post_url 2019-03-22-understanding-emq-x-broker-cluster-concepts %})
- [How to manually setup EMQ X cluster?]({{ site.baseurl }}{% post_url 2019-03-23-how-to-manually-setup-emqx-cluster %})
