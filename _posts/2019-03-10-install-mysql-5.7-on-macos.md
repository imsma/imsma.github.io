---
layout: post
title:  "Install MySQL 5.7 on macOS"
author: sma
categories: [ MySQL, macOS ]
image: assets/images/server-grid.jpg
description: "Install MySQL 5.7 on macOS"
---

## Install Homebrew
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$
```

###  brew services
```
$ brew tap homebrew/services
```

## Install MySQL
```
$ brew info mysql@5.7
```

```
$ brew install mysql@5.7
```


## Load and start the MySQL service
```
$ brew services start mysql@5.7
```

Check of the MySQL service has been loaded 
```
$ brew services list
```

Force link 5.7 version
```
$ brew link mysql@5.7 --force
```

## Verify the installed MySQL instance

```
$ mysql -V
```

## Configuration

Open Terminal and execute the following command to set the root password:
```
mysqladmin -u root password 'yourpassword'
```

## Database Management

You can use [Sequel Pro](http://www.sequelpro.com/) GUI client to manage MySQL

![Sequel Pro]({{ site.baseurl }}/assets/images/sequel-pro.jpg)



### Notes

```
brew services start mysql@5.7
````
Above mentioned instruction is equal to 

```
$ ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

