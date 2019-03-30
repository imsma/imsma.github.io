---
layout: post
title:  "Setup Highly Available Private Networking using Keepalived (VRRP)"
author: sma
categories: [ Scaling,Keepalived,Ubuntu,DistributedSystems]
image: assets/images/posts/cable-colorful-colourful-46218.jpg
description: "Setup Highly Available Private Networking using Keepalived and VRRP (Virtual Router Redundancy Protocol) on Ubuntu 16.04"
tags: [featured]
---

Systems designed for **High Availability (HA)** shall be capable to route workload to another system if first one fails. **keepalived** is used to monitor the *member nodes* and *switchover* to a *standby node* in case of a  on primary / master node.

The primary purpose of **Keepalived** as *routing* software is to facilitate *load balancing* and *high availability* for *linux based systems*. **Keepalived** ensures *high availability* using **VRRP** *(Virtual Router Redundancy Protocol)* protocol. To learn more about **Keepalived**, please consult *[official documentation](http://www.keepalived.org){:target="_blank"}*.

**VRRP** or *Virtual Router Redundancy Protocol* is used to enable *automatic assignment* of available IP address to a host. The concept of *automatic IP assignment* is also known as *shared IP* or *Floating IP*. In this article, I will explain *how to setup Keepalived with VRRP on Ubuntu 16.04 Server*. We will setup *3 redundant servers accessible with single shared / floating IP*.

## How Keepalived and VRRP works

**Keepalived** uses *master / slave* redundancy architecture, participant nodes are defined with *priority* and *node with highest priority is marked as master node* and other are marked as *slave nodes*. *Slave nodes* listen for multicast packets from *master node*, if *slave nodes* fail to receive broadcast from *master node*, the *slave node with highest priority* will be declared as *master node*.

## Our Environment 

Let's assume we have three server nodes with following IP addresses.

{:class="table table-responsive "}
| Server Name | IP Address    |
|-------------|---------------|
| srv-1       | 192.168.0.101 |
| srv-2       | 192.168.0.102 |
| srv-3       | 192.168.0.103 |




## Installing Latest version of Keepalived 2.0.14

The version *v1.2.24* of *Keepalived* in *Ubuntu 16.04.4* default *apt repositories* is outdated, the latest available version of *Keepalived* [*v2.0.14*](https://github.com/acassen/keepalived/releases/tag/v2.0.14) was released on *March 25, 2019*. So in this tutorial we will install the *latest version of Keepalived* from source.

### Setup Build Environment for Keepalived
First of all we will install the *Keepalived* build dependencies.

1. We install `build-essentials` package, this will install different packages required for *build process* in general.
    ```
    sudo apt-get install -y build-essential
    ```
2. Install `libssl-dev` *SSL libraries*, *Keepalived* requires (`libssl-dev`) as build dependency.
    ```
    sudo apt-get install -y libssl-dev
    ```

### Installing Keepalived
First of all, setup **Keepalived** on all three *ubuntu servers* using following steps.

1. Download the [latest](http://www.keepalived.org/download.html) available release of *Keepalived*.
    ```
    wget http://www.keepalived.org/software/keepalived-2.0.14.tar.gz
    ```
2. Extract the downloaded package using `tar` command. 
    ```
    tar xzvf keepalived-2.0.14.tar.gz
    ```
3. Above command will extract the contents of `keepalived-2.0.14.tar.gz` file to directory named `keepalived-2.0.14`, change directory to this one.
    ```
    cd keepalived-2.0.14
    ```
4. Create *Makefiles* file by running the `./configure` shell script.
    ```
    ./configure
    ```
5. Run the `make` command to generate the executable binaries.
    ```
    make
    ```
6. Now run the `make install` command to copy the built artifacts to their proper location.
    ```
    sudo make install
    ```
### Setup Keepalived as systemd service
In this section we will setup *Keepalived* as *systemd service*.

1. Create *systemd service unit file* for *Keepalived* service.
    ```
    sudo nano /etc/systemd/system/keepalived.service
    ```
2. Copy following contents into `keepalived.service` file, save file `(Ctrl + O)` and exit the nano editor `(Ctrl + X)`.
    ```
    #
    #  systemd servive unit file for Keepalived 
    #

    [Unit]
    Description=Keepalived service for High Availability with LVS and VRRP
    After=network.target
    ConditionFileNotEmpty=/etc/keepalived/keepalived.conf

    [Service]
    Type=simple
    # Ubuntu/Debian convention:
    EnvironmentFile=-/etc/default/keepalived
    ExecStart=/usr/local/sbin/keepalived --dont-fork
    ExecReload=/bin/kill -s HUP $MAINPID
    #Define the procedure of killing the processes belonging to the Keepalived service unit.
    KillMode=process

    [Install]
    WantedBy=multi-user.target    
    ```
3. Enable the *Keepalived* service for auto start on system boot.
    ```
    sudo systemctl enable keepalived
    ```
    **Output**
    ```
    Created symlink from /etc/systemd/system/multi-user.target.wants/keepalived.service to /etc/systemd/system/keepalived.service.  
    ```
4. If you try to start *keepalived* service using `sudo service keepalived start` command, it will fail with following status report. Don't worry, this error will get fixed in upcoming sections of this article where we setup configuration files for *Keepalived* *MASTER* and *BACKUP* node. 
    ```sh
    ● keepalived.service - Keepalived service for High Availability with LVS and VRR
    Loaded: loaded (/etc/systemd/system/keepalived.service; enabled; vendor prese
    Active: inactive (dead)
    Condition: start condition failed at Sat 2019-03-30 01:07:18 PKT; 4s ago
            ConditionFileNotEmpty=/etc/keepalived/keepalived.conf was not met    
    ```

## Managing Keepalived Service

Keepalived service can be started, stopped and queried for status using `service` command, in this section we will explore how we can manage *Keepalived* service.

1. *Keepalived* service can be *started* using following command.
    ```
    sudo service keepalived start
    ```
2. *Keepalived* service can be *stopped* using following command.
    ```
    sudo service keepalived stop
    ```
3. *Keepalived* service can be *restarted* using following command.
    ```
    sudo service keepalived restart
    ```
4. We can get current *Keepalived* service as following.
    ```
    sudo service keepalived status
    ```

### Configuring IP forwarding 

To ensure proper network packet forwarding by *Keepalived* service to real servers, we need to enable *IP forwarding* on each participant server. Execute following steps on each server to turn on *IP forwarding*.  

1. Open `sysctl.conf` file for editing.
    ```
    sudo nano /etc/sysctl.conf
    ```
2. Add following line at end of `sysctl.conf` file, save the file and exit editor.
    ```
    net.ipv4.ip_nonlocal_bind=1
    ```
3. Reboot the system so that changes take effect.
4. Execute following command to verify if *IP forwarding* is enabled.
    ```
    sudo sysctl -p /etc/sysctl.conf
    ```
    `sysctl -p` command will return following output, where "`net.ipv4.ip_nonlocal_bind = 1`" confirms that *IP forwarding* is turned on.
    ```
    net.ipv4.ip_nonlocal_bind = 1
    ```

## Setup Master Server Node using Keepalived and VRRP

In this section we will configure **Keepalived** for *master node*. Connect to first server with IP address *192.168.0.101* to configure it as *keepalived master node*.

1. Create `/etc/keepalived/` configuration directory for *keepalived*.
    ```
    sudo mkdir /etc/keepalived/
    ```
1. Create file named `keepalived.conf` in `/etc/keepalived/` directory, this file will hold configurations for our *keepalived* service. 
    ```
    sudo nano /etc/keepalived/keepalived.conf
    ```
2. Copy following configuration to the newly created  `keepalived.conf` file, save file `(Ctrl + O)` and exit the nano editor `(Ctrl + X)`.
    ```
    vrrp_instance VI_1 {
        state MASTER
        interface eth1
        virtual_router_id 88
        priority 200
        advert_int 1
        authentication {
            auth_type PASS
            auth_pass pass1234
        }
        virtual_ipaddress {
            192.168.0.200
        }
    }
    ```
3. Restart the *keepalived* service to make the changes effective.
    ```
    sudo service keepalived restart
    ```

    **IMPORTANT POINTS TO REMEMBER:**
    > Replace "`eth1`" in line "`interface eth1`" with a valid *interface name* as per your system's configuration. You can determine the *interface name* using `ifconfig` command on most linux / unix based systems.

    > Replace *IP Address (192.168.0.200)* in section `virtual_ipaddress` with a valid *IP Address* as per your network environment and subnet.

    > **virtual_router_id** shall remain the same across all participant server regardless of the *MASTER* or *BACKUP* state.

## Setup Backup Server Node using Keepalived and VRRP

In this section we will configure **Keepalived** for first *backup node*. Connect to second server with IP address *192.168.0.102* to configure it as *keepalived backup node*.

1. Open `keepalived.conf` file for editing as following, a new file will be created if doesn't exist.
    ```
    sudo nano /etc/keepalived/keepalived.conf
    ```
2. Add following configuration at the end of `keepalived.conf` file, save the file and exist editor, please note that `state` is set to `BACKUP` for this server.
    ```
    vrrp_instance VI_1 {
        state BACKUP
        interface eth1
        virtual_router_id 88
        priority 100
        advert_int 1
        authentication {
            auth_type PASS
            auth_pass pass1234
        }
        virtual_ipaddress {
            192.168.0.200
        }
    }
    ```
3. Restart the *keepalived* service to make the changes effective.
    ```
    sudo service keepalived restart
    ```

    **IMPORTANT POINTS TO REMEMBER:**
    > Replace "`eth1`" in line "`interface eth1`" with a valid *interface name* as per your system's configuration. You can determine the *interface name* using `ifconfig` command on most linux / unix based systems.

    > Replace *IP Address (192.168.0.200)* in section `virtual_ipaddress` with a valid *IP Address* as per your network environment and subnet.

    > *virtual_router_id* shall remain the same across all participant server regardless of the *MASTER* or *BACKUP* state.

    > `priority` on *backup* / *slave* nodes shall be lower than one defined for *MASTER* node.

## Setup Additional Backup Server Node using Keepalived and VRRP

In this section we will configure **Keepalived** for second *backup node*. Connect to the third server with IP address *192.168.0.103* to configure it as *keepalived backup node*.

1. Open `keepalived.conf` file for editing as following, a new file will be created if doesn't exist.
    ```
    sudo nano /etc/keepalived/keepalived.conf
    ```
2. Add following configuration at the end of `keepalived.conf` file, save the file and exist editor, please note that `state` is set to `BACKUP` for this server and `priority` is even lower than one defined for the first *backup node*
    ```
    vrrp_instance VI_1 {
        state BACKUP
        interface eth1
        virtual_router_id 88
        priority 90
        advert_int 1
        authentication {
            auth_type PASS
            auth_pass pass1234
        }
        virtual_ipaddress {
            192.168.0.200
        }
    }
    ```
3. Restart the *keepalived* service to make the changes effective.
    ```
    sudo service keepalived restart
    ```
    
*Optionally* r*reboot* all three systems to make sure  all the changes have been reloaded.



## Test the Failover for Keepalived and VRRP

We will run simple test using `ping` command to make sure that *highly available environment configured with keepaived and VRRP* is working as required. Please make sure that system used to run following test is not one of three servers used in this tutorial.

1. Open terminal and run `ping` command as following, please note that we are using *virtual ip address (192.168.0.200)* configured for *VRRP router*.
    ```
    ping 192.168.0.200
    ```
    Above command will start pinging the *MASTER node* via *virtual ip address*.

    ```
    PING 192.168.0.200 (192.168.0.200) 56(84) bytes of data.
    64 bytes from 192.168.0.200: icmp_seq=1 ttl=64 time=0.309 ms
    64 bytes from 192.168.0.200: icmp_seq=1 ttl=64 time=0.309 ms
    64 bytes from 192.168.0.200: icmp_seq=1 ttl=64 time=0.309 ms
    64 bytes from 192.168.0.200: icmp_seq=1 ttl=64 time=0.309 ms
    64 bytes from 192.168.0.200: icmp_seq=1 ttl=64 time=0.309 ms
    64 bytes from 192.168.0.200: icmp_seq=1 ttl=64 time=0.309 ms
    ```
2. While the above ping command is running, *turn off* the *MASTER node*, you will notice an increase in latency of ping requests and it will go to normal again. This means the *virtual ip address (192.168.0.200)* is now pointing to *BACKUP node*.
3. If you *turn on* the *MASTER node*, it will take control i.e *virtual ip address (192.168.0.200)* will point back to *MASTER node*.

## Alternative Test using web server
You can also perform the *Keepalived* failover test using *web servers* as following.

1. Deploy simple web server like *apache httpd* on all three servers.
2. Edit `index.html` in `htdocs` directory on all three server and add text `Server 1`, `Server 2` or `Server 3` for respective server.
3. Now open the url with *virtual ip address* (http://192.168.0.200) in a web browser on test machine.
4. By default you will see `Server 1` in web browser.
5. Now *turn off* the *MASTER node* and refresh the browser on test machine now the web page will 
render the text for *BACKUO node* which means *virtual ip address* is now pointing to *BACKUP node*.

This was our simple *Highly Available environment* using *keepalived* and *VRRP* protocol. In upcoming articles I will explain how to setup *High Availability* using *HAProxy*. I hope you enjoyed this article. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!