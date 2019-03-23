---
layout: post
title:  "Install PostgresSQL and PostGIS on Ubuntu"
author: sma
categories: [ PostgresSQL, PostGIS, GIS ]
image: assets/images/posts/gis-map.jpg
tags: [featured]
description: "Install PostgresSQL and PostGIS on Ubuntu"
---

In this article I will explain how to install [PostGIS](https://postgis.net/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

Following is step by step tutorial to setup [PostGIS](https://postgis.net/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

Add Respository to `sources.list`

```
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt xenial-pgdg main" >> /etc/apt/sources.list'
```

Run the following command to add the key to the list of trusted keys.

```
$ wget --quiet -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | sudo apt-key add -
```

Update the package lists.

```
$ sudo apt update
```
Install **PostgresSQL**

```
$ sudo apt install postgresql-10
```
Install **PostGIS 2.4** by running following command.

```
$ sudo apt install postgresql-10-postgis-2.4 
```

Run following command to install **PostGIS** scripts for **PostgresSQL**.

```
$ sudo apt install postgresql-10-postgis-scripts
```