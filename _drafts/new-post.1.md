---
layout: post
title:  "New Post111"
author: sma
categories: [ MQTT,EMQX,IoT,M2M ]
image: assets/images/posts/art-board-game-challenge-163064.jpg
description: "New Post"
tags: [featured]
---

  
How to setup logging for HAProxy using rsyslog?

REF https://www.haproxy.com/blog/introduction-to-haproxy-logging/


```
sudo apt install -y rsyslog
```

configure.... into  `/etc/rsyslog.conf`  or  a new file `/etc/rsyslog.d/haproxy.conf`

```
sudo nano /etc/rsyslog.d/haproxy.conf:
```

copy following contents  into  file, save and exit the file

```
# Collect log with UDP
$ModLoad imudp
$UDPServerAddress 127.0.0.1
$UDPServerRun 514

# Creating separate log files based on the severity
local0.* /var/log/haproxy-traffic.log
local0.notice /var/log/haproxy-admin.log
```

restart the rsyslog service







That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
