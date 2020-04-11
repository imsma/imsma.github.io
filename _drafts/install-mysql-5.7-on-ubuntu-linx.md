---
layout: post
title:  "Install MySQL 5.7 on Ubuntu Server"
author: sma
categories: [ MySQL, Linux, Ubuntu]
image: assets/images/posts/server-grid.jpg
description: "Install MySQL 5.7 on Ubuntu Server"
---

In this article I will guide you how to install [**MySQL 5.7**](https://dev.mysql.com/downloads/mysql/5.7.html) on **Ubuntu Linux Server**.


## Setup APT Repository

??
Download the MySQL apt configuration Debian package officially provided by the MySQL team and install it on your system. For Ubuntu 16.04 and later version’s MySQL 5.7 is available under default apt repositories, so you don’t need to enable additional repository that.
???

```
wget http://repo.mysql.com/mysql-apt-config_0.8.9-1_all.deb
```

and then execute following command.

```
sudo dpkg -i mysql-apt-config_0.8.9-1_all.deb
```

This command will ask you to select MySQL version.


> show the  dialog image


Select the MySQL Server version and press OK.

## Install MySQL Server

Update apt repository

```
sudo apt-get update
```

Install MySQL Server

```
sudo apt-get install mysql-server
```

This will install MySQL Server and at the end will ask to set the root password.


> place relevant images here

At the end  setup will ask to enter password for `root`.


## Verifying the installation with local `mysql` client

Connect `mysql` client using following command

```
mysql -u root -p
```
it will ask for the root password,

run following query to list all databases.

```
show databases;
```

Sample output.

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```


## MySQL Set root password



## Securing the MySQL Server Installation
Run following command to secure MySQL installation.


```
mysql_secure_installation
```






That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
