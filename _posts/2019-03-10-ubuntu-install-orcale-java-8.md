---
layout: post
title:  "Install Oracle Java 8, 9 in Ubuntu 16.04"
author: sma
categories: [ Java, Ubuntu ]
image: assets/images/java-1.jpg
tags: [featured]
---

In this article I will explain how to install [Oracle Java](https://www.oracle.com/java/) on [Ubuntu 16.04 LTS Server](http://releases.ubuntu.com/16.04/). I will explain the installation process of both Oracle Java 8 and 9.

# Install Oracle Java 8, 9 in Ubuntu 16.04

## 1. Add the PPA
Run following command to add Webupd8team<sup>[1]</sup><a href="#ppa"> PPA </a> repository.

```
$ sudo add-apt-repository ppa:webupd8team/java
```

## 2. Update Package Lists
Run following command to update apt package lists.

```
$ sudo apt update
```

## 3. Install Oracle Java 8 installer script
Run following command to run Oracle Java 8 installer script.

```
$ sudo apt install oracle-java8-installer
```

To install Java 9 replace oracle-**java8**-installer with oracle-**java9**-installer.

## 4. Verify Java installation
Run following command to verify **Java 8** installation.

_Verify Java runtime (JRE) version._

```
$ java -version
```

_Verify Java compiler (JDK) version._

```
$ javac -version
```


### Footnotes
1. <a id="ppa">PPA</a> (Personal Package Archives) let you to upload Ubuntu source packages to be built and published as an apt repository [$\hookleftarrow$](#a1)



[1]: https://launchpad.net/~webupd8team/+archive/ubuntu/java
