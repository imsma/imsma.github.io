---
layout: post
title:  "How to manually setup EMQX cluster?"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/posts/agreement-business-clapping-990817.jpg
description: "How to manually setup EMQ X cluster?"
---

In this article we will explore how to setup *EMQ X Broker Cluster* manually using *CLI (Command Line Interface)*. We will create *EMQX broker cluster* of 3 nodes.

## 1. Setup EMQ X Broker Nodes

Follow the guidelines presented in my previous article [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %}){:target="_blank"} to setup **3 EMQ X nodes**. So we have **3 EMQ X nodes** setup as following.

{:class="table table-responsive "}
| # | Node Name         | IP Address     |
|---|-------------------|----------------|
| 1 | emqx@192.168.0.31 | 192.168.0.31   |
| 2 | emqx@192.168.0.32 | 192.168.0.32   |
| 3 | emqx@192.168.0.33 | 192.168.0.33   |

You can set **node name of EMQ X Broker Node** using the `node.name` property defined in `/etc/emqx/emqx.conf` configuration file. Following are detailed steps to set the **node name of EMQ X Broker Node**, follow these steps to setup `node.name` property on each member node.

1. Connect to *system* with **EMQ X Broker Node** and open terminal.
2. Open `/etc/emqx/emqx.conf` file for editing.
```
sudo nano /etc/emqx/emqx.conf
```
3. Press `Ctrl + W`, enter `node.name =` in the *search input field* and press `Enter` key to search the text. This will move the cursor to first occurrence of `'node.name ='`.
```
## Node name.
##
## See: http://erlang.org/doc/reference_manual/distributed.html
##
## Value: <name>@<host>
##
## Default: emqx@127.0.0.1
node.name = emqx@127.0.0.1
```
4. Change the value of `node.name` property to change the **node name of EMQ X Broker Node**.
```
node.name = emqx@192.168.0.31
```

Repeat *Step 1* to *Step 4* for each **EMQ X Broker** node.

Please **note** that you can't change the *node name* of **EMQ X Broker Node** after  joining the cluster.

## Set EMQ X Cluster Cookie

All *nodes* in **EMQ X Cluster** need to have same *cookie value*, this helps **EMQ X** to recognize *nodes* of the same **EMQ X Cluster**.

Repeat following steps for each **EMQ X Broker** node.

1. Open the **EMQ X** *configuration* file `/etc/emqx/emqx.conf`.
```
sudo nano /etc/emqx/emqx.conf
```
2. Press `Ctrl + W`, enter `node.cookie =` in the *search input field* and press `Enter` key to search the text. 
```
## Cookie for distributed node communication.
##
## Value: String
node.cookie = emqxsecretcookie
```
3. Set the value of `node.cookie` property, please remember to set *same value* (`'emqxsecretcookievalue'` in current example) for each member *node* of **EMQ X Cluster**.
```
node.cookie = emqxsecretcookievalue
```
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
3. Set `manual` as value of `cluster.discovery` property.
```
cluster.discovery = manual
```
Please note that default value for `cluster.discovery` property is `manual`.

## Join EMQ X Cluster Manually
We can use `emqx_ctl cluster join` command to join  manually **EMQ X Cluster**. Run `emqx_ctl cluster join` command on *2 nodes* to join the *3rd node* of **EMQ X Cluster** as following.

1. Run `emqx_ctl cluster join` command on *node* with name *emqx@192.168.0.32* to join the first *node* named *emqx@192.168.0.31* as following.
```
emqx_ctl cluster join emqx@192.168.0.31
```
**Output**:
```
Join the cluster successfully.
Cluster status: [{running_nodes,['emqx@192.168.0.31',
                                 'emqx@192.168.0.32']}]
```
2. Now run `emqx_ctl cluster join` command on *node* with name *emqx@192.168.0.33* to join the first *node* named *emqx@192.168.0.31* as following.
```
emqx_ctl cluster join emqx@192.168.0.31
```
**Output**:
```
Join the cluster successfully.
Cluster status: [{running_nodes,['emqx@192.168.0.31','emqx@192.168.0.32',
                                 'emqx@192.168.0.33']}]
```
## Removing a node from EMQ X Cluster
We can remove a *node* from **EMQ X Cluster** using following two methos.

1. A  *member node* of **EMQ X Cluster** can leave can the *cluster* own its own, to remove a *node* own its own, run `leave` command on respective *node*.
```
emqx_ctl cluster leave
```
2. If you want to *remove* a *node* from another *node*, run the `remove` command as following.
```
emqx_ctl cluster remove EMQX_NODE_NAME
```
So if you want to remove node *emqx@192.168.0.32* while you are on node *emqx@192.168.0.31*, run the `remove` command as following.
```
emqx_ctl cluster remove *emqx@192.168.0.32*
```
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

## Other articles in MQTT / EMQ X  series
- [Setup EMQ X broker node on Ubuntu Server]({{ site.baseurl }}{% post_url 2019-03-10-setup-emqx-broker-node-on-ubuntu %})
- [Understanding EMQ X broker cluster concepts]({{ site.baseurl }}{% post_url 2019-03-22-understanding-emq-x-broker-cluster-concepts %})
- [Clustering EMQ X Automatically:static, using a static node list]({{ site.baseurl }}{% post_url 2019-03-23-clustering-emqx-automatically-using-a-static-node-list %})
- [Load Balancer for EMQ X Cluster]({{ site.baseurl }}{% post_url 2019-03-24-load-balancer-for-emq-x-cluster %})
- [EMQ X deployment with HAProxy Load Balancer on private network]({{ site.baseurl }}{% post_url 2019-03-26-emqx-deployment-with-haproxy-load-balancer-on-private-network %})