---
layout: post
title:  "Install etcd as systemd service on Ubuntu Linux server"
author: sma
categories: [ ETCD,DistributedSystems,ServiceDiscovery,KUBERNETES,Cloud ]
image: assets/images/posts/adult-blur-bokeh-712786.jpg
description: "Install etcd (distributed  key-value store) as systemd service on Ubuntu Linux Server"
tags: [featured]
---

**etcd** is a distributed key-value store for distributed system. **etcd** is written in [**Go**](https://golang.org) language. To ensure *high availability*, **etcd** uses [Raft consensus algorithm](https://raft.github.io) for management of replicated log. 

In this article / tutorial we will learn how to install **etcd** node on **Ubuntu Server**.

## Download and extract etcd binaries

Download the [latest release](https://github.com/etcd-io/etcd/releases) of **etcd**, at the time of writing this article the latest release of **etcd** was *[3.3.12](https://github.com/etcd-io/etcd/releases/tag/v3.3.12)*.

```
wget https://github.com/etcd-io/etcd/releases/download/v3.3.12/etcd-v3.3.12-linux-amd64.tar.gz
```

Extract the downloaded file *etcd-v3.3.12-linux-amd64.tar.gz* using `tar`(**t**ape **ar**chive) command.

```
tar xvf etcd-v3.3.12-linux-amd64.tar.gz
```
## Instal the *etcd* and *etcdctl* executables to */usr/local/bin*

We already have extracted the contents of *etcd-v3.3.12-linux-amd64.tar.gz* to *etcd-v3.3.12-linux-amd64* directory, now let's install the extracted binaries to */usr/local/bin* directory.

1. Move the `etcd` binary to `/usr/local/bin` directory.
    ```
    sudo mv ./etcd-v3.3.12-linux-amd64/etcd /usr/local/bin 
    ```
2. Move the `etcdctl` binary to `/usr/local/bin` directory.
    ```
    sudo mv ./etcd-v3.3.12-linux-amd64/etcdctl /usr/local/bin 
    ```

### Verify the etcd installation
We will verify the correct installation of **etcd** by running the `etcd --version` command.

```
etcd --version
```
**Output** of the `etcd --version` command.

```
etcd Version: 3.3.12
Git SHA: d57e8b8
Go Version: go1.10.8
Go OS/Arch: linux/amd64
```

Additionally verify installation of **etcdctl** by running `etcdctl --version` command.

```
etcdctl --version
```

**Output** of `etcd --version` command.

```
etcdctl version: 3.3.12
API version: 2
```

## Setup etcd *Configuration* and *Data* Directories

In this section we will setup directories to store **etcd** *configuration* and *data* files.

2. Create **etcd** *configuration directory*.
    ```
    sudo mkdir /etc/etcd
    ```
2. Create **etcd** *data directory*.
    ```
    sudo mkdir -p /var/lib/etcd/
    ```

## Install etcd as *systemd* service

In this section we will setup **etcd** as **systemd daemon** so that i can be started with *system startup*.

Before configuring **etcd** as *systemd service*, let's create *etcd system  group and user*. 

1. Create **etcd** *system group*.
    ```
    sudo groupadd --system etcd
    ```
2. Create **etcd** *system* user with *nologin shell* and add it to **etcd** system group created in previous step.
    ```
    sudo useradd -s /sbin/nologin --system -g etcd etcd
    ```
3. Grant the *ownership* of `/var/lib/etcd/` to *etcd* user.
    ```
    sudo chown -R etcd:etcd /var/lib/etcd/
    ```

Now let's setup **etcd** *service daemon*.
1. Create **etcd** *systemd service* unit file.
    ```
    sudo nano /etc/systemd/system/etcd.service
    ```
2. Put following contents in to newly created *etcd.service* unit file.

    ```
    [Unit]
    Description=etcd service
    Documentation=https://github.com/etcd-io/etcd
    After=network.target

    [Service]
    User=etcd
    Type=notify
    Environment=ETCD_DATA_DIR=/var/lib/etcd
    Environment=ETCD_NAME=%m
    ExecStart=/usr/local/bin/etcd
    Restart=always
    RestartSec=10s
    LimitNOFILE=40000

    [Install]
    WantedBy=multi-user.target
    ```

3. Run the `systemctl daemon-reload` command to load our newly created *unit file* for **etcd** *systemd service*. This will notify *systemd* about newly created *etcd.service* service.
    ```
    sudo systemctl daemon-reload
    ```
**Note** You will need to execute `ystemctl daemon-reload` command whenever you *modify* the *etcd.service* unit file, otherwise `systemctl start` and `systemctl enable` commands will fail due to mismatch between *etcd.service* unit file on disk and currently loaded state of *systemd*.
4. Start the **etcd** service using `systemctl start` command.
    ```
    sudo systemctl start etcd.service
    ```
5. Finally, we will use the `systemctl enable` command to make sure that the *etcd* service starts on *system boot*.
    ```
    sudo systemctl enable etcd.service
    ```

If `systemctl enable` command was successful the output will be as following.

```
Created symlink from /etc/systemd/system/multi-user.target.wants/etcd.service to /etc/systemd/system/etcd.service.
```

### Check etcd *systemd* service status
We can check current status of *etcd* service using `systemctl status` command. Run `systemctl status` command to get current status of *etcd service*.

```
sudo systemctl status etcd.service
```

This will **Output** current status of *etcd service*.

```
● etcd.service - etcd service
   Loaded: loaded (/etc/systemd/system/etcd.service; disabled; vendor preset: enabled)
   Active: active (running) since Sun 2019-03-24 23:41:13 PKT; 28min ago
     Docs: https://github.com/etcd-io/etcd
 Main PID: 6442 (etcd)
    Tasks: 8
   Memory: 5.0M
      CPU: 4.439s
   CGroup: /system.slice/etcd.service
           └─6442 /usr/local/bin/etcd

Mar 24 23:41:13 sb-emq-1 etcd[6442]: 8e9e05c52164694d received MsgVoteResp from 8e9e05c52164694d at term 2
Mar 24 23:41:13 sb-emq-1 etcd[6442]: 8e9e05c52164694d became leader at term 2
Mar 24 23:41:13 sb-emq-1 etcd[6442]: raft.node: 8e9e05c52164694d elected leader 8e9e05c52164694d at term 2
Mar 24 23:41:13 sb-emq-1 etcd[6442]: setting up the initial cluster version to 3.3
Mar 24 23:41:13 sb-emq-1 etcd[6442]: set the initial cluster version to 3.3
Mar 24 23:41:13 sb-emq-1 systemd[1]: Started etcd service.
Mar 24 23:41:13 sb-emq-1 etcd[6442]: enabled capabilities for version 3.3
Mar 24 23:41:13 sb-emq-1 etcd[6442]: published {Name:ec7c4a15dc9eff6480150aff5c8a8a5f ClientURLs:[http://localhost:2379]} to cluster cdf818194e3a8c32
Mar 24 23:41:13 sb-emq-1 etcd[6442]: ready to serve client requests
Mar 24 23:41:13 sb-emq-1 etcd[6442]: serving insecure client requests on 127.0.0.1:2379, this is strongly discouraged!
```
## Verify the ctcd cluster status

We can get details about **etcd** cluster using `etcdctl member list` command.

```
etcdctl member list
```

Currently we have setup just one **etcd** node, so we will get following output.

```
8e9e05c52164694d: name=ec7c4a15dc9eff6480150aff5c8a8a5f peerURLs=http://localhost:2380 clientURLs=http://localhost:2379 isLeader=true
```

## Summary
We have installed and setup *single node* **etcd** cluster, in my upcoming article I will explain how to setup **etcd** *multi node* cluster.

Thanks for reading. I hope you enjoyed this article. If you like this article, have any questions or suggestions please let us know in the comments section.

Happy Learning!