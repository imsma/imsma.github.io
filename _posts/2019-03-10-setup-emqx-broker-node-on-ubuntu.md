---
layout: post
title:  "Setup EMQ X broker node on Ubuntu Server"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/posts/city-downtown-light.jpg
description: "Setup EMQX broker node on Ubuntu Server"
---

[EMQX](https://www.emqx.io/) a distributed, highly available and highly scalable message broker. **EMQX** is designed and built using [Erlang/OTP](https://github.com/erlang/otp), and supports all major **IoT** protocols to develop scalable solutions for **M2M** and **mobile application** messaging.

Following steps will help you setup **EMQX** broker on **Ubuntu Server**.

## Install erlang 

[Click here]({{ site.baseurl }}{% post_url 2019-03-10-install-erlang %}) to view detailed article on how to install erlang on different operating systems.

This article focuses on *Ubuntu Server 16.04 LTS*, to install *erlang* on *Ubuntu* run following command.

```
$ apt-get install erlang
```

## Download the deb package

```
$ wget https://www.emqx.io/downloads/broker/v3.0.0/emqx-ubuntu16.04-v3.0.0_amd64.deb
```


## Install the EMQX deb package

```
$ sudo dpkg -i emqx-ubuntu16.04-v3.0.0_amd64.deb
```


## Install lksctp-tools library

Erlang/OTP R19 depends on `lksctp-tools library` so let's install it.

```
$ sudo apt-get install lksctp-tools
```

## EMQ X Broker Services

Following sections will show how to start, stop, restart and check  *EMQ X broker* services status. 

### Check Status of EMQ X Broker Service

Run the following command to check current status of *EMQ X broker* service.

```
service emqx status
```

**Output** 

```
● emqx.service - LSB: Erlang MQTT Broker
   Loaded: loaded (/etc/init.d/emqx; bad; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:systemd-sysv-generator(8)
```

### Start EMQ X Broker Service

Run following command to start *EMQ X broker* service.

```
sudo service emqx start
```

Run the `status` command as following to verify if service started successfully. 

```
service emqx status
```

**Output** 

```
● emqx.service - LSB: Erlang MQTT Broker
   Loaded: loaded (/etc/init.d/emqx; bad; vendor preset: enabled)
   Active: active (running) since Thu 2019-03-21 20:52:43 PKT; 6s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 3231 ExecStart=/etc/init.d/emqx start (code=exited, status=0/SUCCESS)
    Tasks: 1
   Memory: 236.0K
      CPU: 1.412s
   CGroup: /system.slice/emqx.service
           └─3297 /usr/lib/emqx/erts-10.2/bin/epmd -daemon

Mar 21 20:52:32 u-srv systemd[1]: Starting LSB: Erlang MQTT Broker...
Mar 21 20:52:32 u-srv emqx[3231]:  * Starting emqx
Mar 21 20:52:33 u-srv su[3298]: Successful su for emqx by root
Mar 21 20:52:33 u-srv su[3298]: + ??? root:emqx
Mar 21 20:52:33 u-srv su[3298]: pam_unix(su:session): session opened for user em
Mar 21 20:52:43 u-srv emqx[3231]: emqx 3.0 is started successfully!
Mar 21 20:52:43 u-srv emqx[3231]:    ...done.
Mar 21 20:52:43 u-srv systemd[1]: Started LSB: Erlang MQTT Broker.
```

### Stop EMQ X Broker Service

Run following command to stop *EMQ X broker* service.

```
sudo service emqx stop
```

Run the `status` command as following to verify if service stopped successfully. 

```
service emqx status
```

**Output** 

```
● emqx.service - LSB: Erlang MQTT Broker
   Loaded: loaded (/etc/init.d/emqx; bad; vendor preset: enabled)
   Active: inactive (dead) since Thu 2019-03-21 21:04:26 PKT; 6s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 3844 ExecStop=/etc/init.d/emqx stop (code=exited, status=0/SUCCESS)
  Process: 3231 ExecStart=/etc/init.d/emqx start (code=exited, status=0/SUCCESS)
    Tasks: 1
   Memory: 240.0K
      CPU: 2.708s
   CGroup: /system.slice/emqx.service
           └─3297 /usr/lib/emqx/erts-10.2/bin/epmd -daemon

Mar 21 20:52:33 u-srv su[3298]: + ??? root:emqx
Mar 21 20:52:33 u-srv su[3298]: pam_unix(su:session): session opened for user em
Mar 21 20:52:43 u-srv emqx[3231]: emqx 3.0 is started successfully!
Mar 21 20:52:43 u-srv emqx[3231]:    ...done.
Mar 21 20:52:43 u-srv systemd[1]: Started LSB: Erlang MQTT Broker.
Mar 21 21:04:09 u-srv systemd[1]: Stopping LSB: Erlang MQTT Broker...
Mar 21 21:04:09 u-srv emqx[3844]:  * Stopping emqx
Mar 21 21:04:10 u-srv emqx[3844]: ok
Mar 21 21:04:26 u-srv emqx[3844]:    ...done.
Mar 21 21:04:26 u-srv systemd[1]: Stopped LSB: Erlang MQTT Broker.
```

### Restart EMQ X Broker Service

Run following command to restart *EMQ X broker* service.

```
sudo service emqx restart
```

Run the `status` command as following to verify if service restarted successfully. 

```
service emqx status
```

**Output** 

```
● emqx.service - LSB: Erlang MQTT Broker
   Loaded: loaded (/etc/init.d/emqx; bad; vendor preset: enabled)
   Active: active (running) since Thu 2019-03-21 21:05:15 PKT; 4s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 3844 ExecStop=/etc/init.d/emqx stop (code=exited, status=0/SUCCESS)
  Process: 4020 ExecStart=/etc/init.d/emqx start (code=exited, status=0/SUCCESS)
    Tasks: 1
   Memory: 256.0K
      CPU: 1.669s
   CGroup: /system.slice/emqx.service
           └─3297 /usr/lib/emqx/erts-10.2/bin/epmd -daemon

Mar 21 21:05:04 u-srv systemd[1]: Starting LSB: Erlang MQTT Broker...
Mar 21 21:05:04 u-srv emqx[4020]:  * Starting emqx
Mar 21 21:05:06 u-srv su[4086]: Successful su for emqx by root
Mar 21 21:05:06 u-srv su[4086]: + ??? root:emqx
Mar 21 21:05:06 u-srv su[4086]: pam_unix(su:session): session opened for user em
Mar 21 21:05:15 u-srv emqx[4020]: emqx 3.0 is started successfully!
Mar 21 21:05:15 u-srv emqx[4020]:    ...done.
Mar 21 21:05:15 u-srv systemd[1]: Started LSB: Erlang MQTT Broker.
```
## EMQ X Web Dashboard 
Default port for *EMQ X Web Dashboard* is `18083`, to view *EMQ X Web Dashboard* open following url in any web browser. Please replace `IP_ADDRESS` with valid *IP Address* of the machine where you installed the *EMQ X broker*.  

Default credentials for *EMQ X Web Dashboard*  are as following.

 |**User Name**| :| *admin* | 
 |**Password** | :| *public* |


1. If you are on the same machine where you installed *EMQ X broker*.
```
http://localhost:18083
```

2. If you are on a different / remote machine, replace the `IP_ADDRESS` with valid *IP Address* of the machine where you installed the *EMQ X broker*.
```
http://IP_ADDRESS:18083
```

![EMQ X Web Dashboard login page]({{ site.baseurl }}/assets/images/posts/emq-x-web-dashboard-login.jpg)
Screenshot: *EMQ X Web Dashboard Login Screen*

![EMQ X Web Dashboard]({{ site.baseurl }}/assets/images/posts/emq-x-web-dashboard.jpg)
Screenshot: *EMQ X Web Dashboard UI*

## Configuration

Configuration, log and data files are located in following directories.

{:class="table table-responsive "}
| File Location             | Description                               |
| ------------------------- | ---------------------------------------   |
| /etc/emqx/emqx.conf       | EMQ X Broker's configuration file         |
| /etc/emqx/plugins/*.conf  | EMQ X Plugins's configuration files       |
| /var/log/emqx             | EMQ X Broker's log files                  |
| /var/lib/emqx/            | EMQ X Broker's data files                 |


## EMQ X TCP Ports

Following table shows the default *TCP Ports* used by *EMQ X broker*.

{:class="table table-responsive "}
| Port   | Description                          |
| ------ | ----------------------------------   |
| 1883   | TCP port for MQTT protocol           |
| 8883   | SSL Port TCP port for MQTT protocol  |
| 8083   | Port for MQTT over WebSocket         |
| 8084   | MQTT/WebSocket/SSL Port              |
| 8080   | HTTP Port Management API             |
| 18083  | Port for Web Dashboard               |

### Configuring EMQ X Broker's TCP ports 

All of the default TCP ports for *EMQ X broker* can be configured in `etc/emqx.config` file.

#### Configuring TCP port for MQTT protocol 

*TCP port for MQTT protocol* can be configured using `listener.tcp.external` property in  `etc/emqx.config`.

```
## MQTT/TCP - External TCP Listener for MQTT Protocol

## listener.tcp.$name is the IP address and port that the MQTT/TCP
## listener will bind.
##
## Value: IP:Port | Port
## 
## Examples: 1883, 127.0.0.1:1883, ::1:1883

listener.tcp.external = 0.0.0.0:1883
```

#### Configuring SSL Port TCP port for MQTT protocol

 *SSL Port TCP port for MQTT protocol* can be configured using `listener.ssl.external` property in  `etc/emqx.config`.

```
## listener.ssl.$name is the IP address and port that the MQTT/SSL
## listener will bind.
##
## Value: IP:Port | Port
## 
## Examples: 8883, 127.0.0.1:8883, ::1:8883

listener.ssl.external = 8883
```

#### Configuring Port for External WebSocket listener for MQTT protocol

 *Port for External WebSocket listener for MQTT protocol* can be configured using `listener.ws.external` property in  `etc/emqx.config`.

```
## External WebSocket listener for MQTT protocol

## listener.ws.$name is the IP address and port that the MQTT/WebSocket
## listener will bind.
##
## Value: IP:Port | Port
## 
## Examples: 8083, 127.0.0.1:8083, ::1:8083
listener.ws.external = 8083
```

#### Configuring Port for External WebSocket/SSL listener for MQTT Protocol

 *Port for External WebSocket/SSL listener for MQTT Protocol* can be configured using `listener.wss.external` property in  `etc/emqx.config`.

```
## External WebSocket/SSL listener for MQTT Protocol

## listener.wss.$name is the IP address and port that the MQTT/WebSocket/SSL
## listener will bind.
##
## Value: IP:Port | Port
## 
## Examples: 8084, 127.0.0.1:8084, ::1:8084
listener.wss.external = 8084
```



That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!

## Other articles in MQTT / EMQ X  series
- [Understanding EMQ X broker cluster concepts]({{ site.baseurl }}{% post_url 2019-03-22-understanding-emq-x-broker-cluster-concepts %})
- [How to manually setup EMQ X cluster?]({{ site.baseurl }}{% post_url 2019-03-23-how-to-manually-setup-emqx-cluster %})
- [Clustering EMQ X Automatically:static, using a static node list]({{ site.baseurl }}{% post_url 2019-03-23-clustering-emqx-automatically-using-a-static-node-list %})
- [Load Balancer for EMQ X Cluster]({{ site.baseurl }}{% post_url 2019-03-24-load-balancer-for-emq-x-cluster %})
- [EMQ X deployment with HAProxy Load Balancer on private network]({{ site.baseurl }}{% post_url 2019-03-26-emqx-deployment-with-haproxy-load-balancer-on-private-network %})