---
layout: post
title:  "Install MySQL 5.7 on macOS"
author: sma
categories: [ MySQL, macOS ]
image: assets/images/server-grid.jpg
description: "Install MySQL 5.7 on macOS"
---

In this article I will guide you how to install [**MySQL 5.7**](https://dev.mysql.com/downloads/mysql/5.7.html) on **macOS** using [**Homebrew**](https://brew.sh) *package manager* for **macOS**.

## Install Homebrew

Install [**Homebrew**](https://brew.sh) if you do not have already installed.

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

###  Install brew services 

Install **brew services** using following terminal command.

```
$ brew tap homebrew/services
```

## Install MySQL 5.7

List the default **MySQL** available formulae in main repository.

```
$ brew info mysql@5.7
```

Install **MySQL 5.7** using `brew install` command.

```
$ brew install mysql@5.7
```


## Load and start the MySQL service

Start **MySQL** service using `brew services start` command.

```
$ brew services start mysql@5.7
```

Check of the MySQL service has been loaded.

```
$ brew services list
```

Force link 5.7 version
```
$ brew link mysql@5.7 --force
```

## Verify the installed MySQL instance

Verify the newly installed instance of **MySQL** server.

```
$ mysql -V
```
The output of `mysql -V` will be like following one.

```
mysql  Ver 14.14 Distrib 5.7.24, for osx10.14 (x86_64) using  EditLine wrapper
```

## Configuration of MySQL 5.7 server

Open terminal and execute the following command to set the root password.

```
mysqladmin -u root password 'yourpassword'
```

## MySQL 5.7 Database Management

You can use [Sequel Pro](http://www.sequelpro.com/) GUI client to manage MySQL

![Sequel Pro]({{ site.baseurl }}/assets/images/sequel-pro.jpg)

### Notes

```
brew services start mysql@5.7
````
Above mentioned instruction is same as executing following commands individually.

```
$ ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
