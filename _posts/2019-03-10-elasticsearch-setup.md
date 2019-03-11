---
layout: post
title:  "Setup Elasticsearch on Ubuntu Server"
author: sma
categories: [ Elasticsearch, Ubuntu ]
image: assets/images/anthony-martino-335460-unsplash.jpg
---

In this article I will explain how to install [Elasticsearch](https://www.elastic.co/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

Following is step by step tutorial to setup [Elasticsearch](https://www.elastic.co/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/).

Run the following command to add the key to the list of trusted keys.

```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

Add the [Elasticsearch](https://www.elastic.co/) repository.
```
$ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
```

Update the package lists.

```
$ sudo apt-get update
```

Run following command to install **Elasticsearch** using apt package manager.

```
$ sudo apt-get install elasticsearch
```