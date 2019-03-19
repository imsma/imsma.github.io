---
layout: post
title:  "How to change Ubuntu system's Hostname"
author: sma
categories: [ Ubuntu ]
image: assets/images/analogue-blur-business-159282.jpg
description: "How to change Hostname / Computer Name in Ubuntu"

---

In this article I will explain how you can change ubuntu host name temporarily and permanently.

## Set the system's host name
Run following command from terminal to change system's host name. Replace *HOST_NAME* with a name you want to set as new host name. This will change host name temporarily and will be reverted to original host name on system reboot. In next section I will explain how to change host name permanently. 

```
hostname HOST_NAME
```

## Change host name permanently
To change system's host name permanently, we need to edit `hostname` and `hosts` files located in `/etc` directory.

### Change `hostname` file
Run following command to change `hostname` file.

```
sudo nano /etc/
```

Above command will open `hostname` containing current host name. 

```
CURRENT_HOST_NAME
```

Replace `CURRENT_HOST_NAME` with `NEW_HOST_NAME`.

```
NEW_HOST_NAME
```

Save the file by pressing `Ctrl + O` and exit the editor by pressing `Ctrl + X` keys.

### Change `hosts` file

Open the `hosts` file for editing.

```
sudo nano /etc/hosts
```
`hosts` file contains mapping of hostnames to IP addresses.

```

127.0.0.1       localhost
127.0.1.1       CURRENT_HOST_NAME

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

```

Change the `CURRENT_HOST_NAME` with `NEW_HOST_NAME`, after making these changing the `hosts` file will look as following.

```

127.0.0.1       localhost
127.0.1.1       NEW_HOST_NAME

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

```

Save the file by pressing `Ctrl + O` and exist the editor by pressing `Ctrl + X` keys.

Finally reboot the system to load the new configurations.

```
sudo reboot
```

Thank you for reading. Please let me know (in comments section) if you have any questions.

