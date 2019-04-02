---
layout: post
title:  "How to install PostgresSQL 11 with PostGIS 2.5 on Ubuntu Linux?"
author: sma
categories: [ PostgresSQL, PostGIS, GIS ]
image: assets/images/posts/gis-map.jpg
description: "How to install PostgresSQL 11 with PostGIS 2.5 on Ubuntu Linux?"
---

**PostGIS** is an extension for **PostgresSQL** to enable *Spatial and Geographic* features, *PostGIS* enables *PostgresSQL* to execute *location queries*.

In this article I will explain how to install [PostGIS](https://postgis.net/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

## Installing PostgresSQL 11

Please follow my tutorial [How to install PostgresSQL 11 on Ubuntu Server?]({{ site.baseurl }}{% post_url 2019-03-10-install-postgressql-11-on-ubuntu-server %}) for detailed instruction to install *PostgresSQL 11* on *Ubuntu linux*.


Install **PostGis 2.5** for *PostgresSQL 11* using *apt package manager* as following.

```
sudo apt install postgresql-11-postgis-2.5 
```

This was a step by step guide to install *PostGIS 2.5 with PostgresSQL 11 server* on *Ubuntu linux* machine. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning.