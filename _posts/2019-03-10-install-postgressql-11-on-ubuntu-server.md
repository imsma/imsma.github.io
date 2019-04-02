---
layout: post
title:  "How to install PostgresSQL 11 on Ubuntu Linux Server?"
author: sma
categories: [ PostgresSQL, Ubuntu ]
image: assets/images/posts/computer-connection-data-1181675.jpg
description: "How to install PostgresSQL 11 on Ubuntu Linux Server?"
---

In this article I will explain how to install [PostgresSQL 11](https://www.postgresql.org/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

Following is step by step tutorial to setup [PostgresSQL 11](https://www.postgresql.org/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).


## PostgresSQL 11 Installation Prerequisites 

Before initiating the *installation process of PostgresSQL 11* we need to complete following prerequisites steps.

Add `sources.list` entry for *PostgresSQL* by creating `/etc/apt/sources.list.d/pgdg.list` file. Add following contents to the newly created file by replacing *UBUNTU_VERSION* with the *codename of installed Ubuntu release*.

```
deb http://apt.postgresql.org/pub/repos/apt/ UBUNTU_VERSION-pgdg main
```

*UBUNTU_VERSION* or *LSB (Linux Standard Base)* mentioned above can be retrieved using using `lsb_release` command as following.

`lsb_release -cs` 

**Where**
- `'c'` is for codename.
- `'s'` is for short output.

Using `lsb_release` and `echo` commands, we can automate the file creation process as following.

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -sc)-pgdg main" | sudo tee  /etc/apt/sources.list.d/pgdg.list
```

Run following command to verify contents of repository file.

```
$ cat /etc/apt/sources.list.d/pgdg.list
```


Next  we need to import the *repository signing key* and update the *apt package list* as following.

```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

**Output**

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  4812  100  4812    0     0   2868      0  0:00:01  0:00:01 --:--:--  2867
OK
```

Run the `apt-get update` command to update package list.

```
sudo apt-get update
```


## Install PostgreSQL 11 Server


Install **PostgresSQL 11** using apt package manager.

```
$ sudo apt -y install postgresql-11
```

## Allow  access to PostgreSQL from remote hosts
By default access to **PostgresSQL** server is allowed only from local host. Here we will make the required configurations to allow remote access to newly installed **PostgresSQL** server.

Run [Socket Statistics (ss) ](https://www.rootusers.com/21-ss-command-examples-in-linux/) command to show IP binding against port 5432 _(default PostgresSQL tcp port)_.

```
sudo ss -tunelp | grep 5432
```

Open the **PostgresSQL** configuration file in editor to make required changes for remote access.

```
sudo nano /etc/postgresql/11/main/postgresql.conf
```

Look for the `listen_addresses` configuration entry under `CONNECTIONS AND AUTHENTICATION` section, `listen_addresses` property specifies TCP/IP address(es) where *PostgresSQL server* will listen for incoming client connections. Set `'*'` as the value of `listen_addresses` property to bind *PostgresSQL* server with all available IPs on all network interfaces of this system.

```
listen_addresses = '*'
```

Alternatively, you can specify a particular *IP Address* as following.

```
listen_addresses = '192.168.17.12'
```

To allow remote connection to *PostgresSQL server*

*PostgresSQL server* uses `pg_hba.conf` *(Postgres Host Based Authentication)* configuration file to manage client authentication, to allow *remote PostgresSQL client* to connect and authenticate we need to add entry for that that client in `pg_hba.conf` file.

1. Open `pg_hba.conf` file for editing as following.
    ```
    sudo nano /etc/postgresql/11/main/pg_hba.conf 
    ```
2. Add entry for a specific client IP or range as following.
    ```
    host    all             all             192.168.0.75/32              md5
    ```
    Or you may add the the complete range *192.168.0.X* as following.
    ```
    host    all             all             192.168.0.0/32              md5
    ```    

Restart **PostgresSQL** service to load the updated *configuration*.

```
sudo systemctl restart postgresql
```

Run following command to confirm the bind address for **PostgresSQL** server.

```
$ sudo ss -tunelp | grep 5432
tcp   LISTEN  0       128    0.0.0.0:5432         0.0.0.0:*      users:(("postgres",pid=16066,fd=3)) uid:111 ino:42972 sk:8 <->                  tcp   LISTEN  0       128    [::]:5432            [::]:*      users:(("postgres",pid=16066,fd=6)) uid:111 ino:42973 sk:9 v6only:1 <->
```

Verify the successful start of *PostgresSQL* service by executing following command.

```
sudo service postgresql status
```

**Output**

```
● postgresql.service - PostgreSQL RDBMS
   Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
   Active: active (exited) since Wed 2019-04-03 01:31:42 PKT; 39min ago
  Process: 1763 ExecReload=/bin/true (code=exited, status=0/SUCCESS)
  Process: 6940 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
 Main PID: 6940 (code=exited, status=0/SUCCESS)

Apr 03 01:31:42 u-srv-1 systemd[1]: Starting PostgreSQL RDBMS...
Apr 03 01:31:42 u-srv-1 systemd[1]: Started PostgreSQL RDBMS.
```

## Updating Password of the default admin of  *PostgresSQL* Server

The default admin user for *PostgresSQL* server is named as *postgres*, execute following commands to set password for *postgres* user.

1. Switch the current user to *postgres* using `su` command.
    ```
    sudo su - postgres
    ```
2. Update the password for *postgres* user by executing `alter user postgres with password` SQL statement with `psql` *PostgresSQL client* as following.
    ```
    postgres@os1:~$ psql -c "alter user postgres with password 'StrongPassword'"
    ```
    **Ouput**
    ```
    ALTER ROLE
    ```
3. All done, now run `logout` command to return back to your own logged-in user.
    ```
    logout
    ```

This was a step by step guide to install *PostgresSQL server* on *Ubuntu linux* machine. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning.

## Related Articles
- [How to install PostgresSQL 11 with PostGIS 2.5 on Ubuntu Linux?]({{ site.baseurl }}{% post_url 2019-03-10-install-postgressql-and-postgis-on-ubuntu %})