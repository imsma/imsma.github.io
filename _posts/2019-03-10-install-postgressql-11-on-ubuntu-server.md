---
layout: post
title:  "Install PostgresSQL 11 on Ubuntu Server"
author: sma
categories: [ PostgresSQL, Ubuntu ]
image: assets/images/posts/computer-connection-data-1181675.jpg
description: "Install PostgresSQL 11 on Ubuntu Server"
---

In this article I will explain how to install [PostgresSQL 11](https://www.postgresql.org/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

Following is step by step tutorial to setup [PostgresSQL 11](https://www.postgresql.org/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).


#### Add PostgreSQL 11 APT repository
Run following comand from shell to add PostgreSQL 11 APT repository.

```
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

```

Add repository contents to your Ubuntu 18.04/16.04 system.

```
$ RELEASE=$(lsb_release -cs)
$ echo "deb http://apt.postgresql.org/pub/repos/apt/ ${RELEASE}"-pgdg main | sudo tee  /etc/apt/sources.list.d/$ pgdg.list
```

Run following command to verify contents of repository file.

```
$ cat /etc/apt/sources.list.d/pgdg.list
```

#### Install PostgreSQL 11
Update the apt package maanger repository by running following command.

```
$ sudo apt update
```

Install **PostgresSQL 11** using apt package manager.

```
$ sudo apt -y install postgresql-11
```


#### Allow  access to PostgreSQL from remote hosts
By default access to **PostgresSQL** server is allowed only from local host. Here we will make the required configurations to allow remote access to newly installed **PostgresSQL** server.

Run [Socket Statistics (ss) ](https://www.rootusers.com/21-ss-command-examples-in-linux/) command to show IP binding against port 5432 _(default PostgresSQL tcp port)_.

```
$ sudo ss -tunelp | grep 5432
```

Open the **PostgresSQL** configuration file in editor to make required changes for remote access.

```
$ sudo nano /etc/postgresql/11/main/postgresql.conf
```


`listen_addresses` configuration `CONNECTIONS AND AUTHENTICATION` section specifies TCP/IP address(es) where **PostgresSQL** server will listen for incoming client connections. Set `'*'` for `listen_addresses` property to bind **PostgresSQL** server with all available IP interfaces.

```
listen_addresses = '*'
```

You can also specify a particular IP Address as following.

```
listen_addresses = '192.168.17.12'
```

Restart **PostgresSQL** service to load the updated _configuration_.

```
$ sudo systemctl restart postgresql
```

Run following command to confirm the bind address for **PostgresSQL** server.

```
$ sudo ss -tunelp | grep 5432
tcp   LISTEN  0       128    0.0.0.0:5432         0.0.0.0:*      users:(("postgres",pid=16066,fd=3)) uid:111 ino:42972 sk:8 <->                  tcp   LISTEN  0       128    [::]:5432            [::]:*      users:(("postgres",pid=16066,fd=6)) uid:111 ino:42973 sk:9 v6only:1 <->
```

#### Set a password for the default **PostgresSQL** admin user

```
$ sudo su - postgres
postgres@os1:~$ psql -c "alter user postgres with password 'StrongPassword'"
ALTER ROLE
```
