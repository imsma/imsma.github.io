---
layout: post
title:  "How to run your Java / Spring Boot App / Jar / WAR as Systemd Service on Linux"
author: sma
categories: [ Java,SpringBoot,Linux ]
image: assets/images/posts/java-1.jpg
description: "How to run your Java / Spring Boot App / Jar / WAR as Systemd Service on Linux?"
---

This article walks you through the process of deploying **Java** *JAR* or *WAR* package file on *linux* operating system  as *systemd service*. Please note that we will discuss the generic method of *JAR / WAR* deployment regardless of the underlaying framework or programming language available on JVM stack, so this guide is applicable to deployment of artifacts produced with frameworks like *Spring*, *Spring Boot*, *Grails* and *Dropwizard* and languages including *Java* , *Scala* and *Kotlin*. 

## What is Systemd?
Systemd is a system and service manager for Linux operating systems

In **Linux operating systems**, *Systemd* is used to manage system and services. *Systemd* uses configuration files known as *systemd unit files* to create a *linux daemon*. Some of common *systemd unit types* include.
- **Service Unit** : A unit file (with *.service* file extension) used to create system service.
- **Automount Unit** : A unit file (with *.automount* file extension) used to create auto-mount point.
- **Timer Unit** : A unit file (with *.timer* file extension) used to create systemd timer.
- **Socket Unit** : A unit file (with *.socket* file extension) used to create a socket.

## Java Spring Boot App as Systemd Service

With some high level overview of *systemd*, let's start our journey of *deploying a Java Spring Boot app as systemd service*. We will start by creating  *deployment directory structure*.

## Deployment directory structure

We will put our binaries *(JAR / WAR)* in `/usr/local/bin` directory.

```
sudo cp myapp.jar /usr/local/bin 
```

We will place our configuration files under `/etc/` directory, let's create *configuration directory* for our app.

```
sudo mkdir /etc/myapp
```

## Environment file for systemd service

We can set *environment variables* for *systemd service unit* using `Environment` configuration option. To avoid lengthy list of *environment variables* within *service unit file*, we can store all the *environment variables* in a separate *environment file* that can be pointed using *EnvironmentFile* directive in *service unit file*. The key advantage of using *environment file* is dynamic injection of configuration without having to run *daemon-reload* command. Let's create the *environment file* for our service with the service configuration directory `/etc/myapp`.

```
sudo nano /etc/myapp/environmentfile
```

Copy following contents into newly created *environment file*.

```
ROOT_DIR=/usr/local/bin
EXEC_JAR="myapp.jar"
JAVA_OPTS="-Xmx128m"
WEB_SERVER_PORT="8088"
USER="myapp"
```

## Creating the system group and user for systemd service

In this section we will create *system user* to run our *systemd service*.

1. Create **myapp** *system group*.
    ```
    sudo groupadd --system myapp
    ```
2. Create **myapp** *system* user with *nologin shell* and add it to **myapp** system group created in previous step.
    ```
    sudo useradd -s /sbin/nologin --system -g myapp myapp
    ```
## Creating the systemd service unit file for Java / Spring Boot App

In this section we will create the *systemd service unit file* for our Java Spring Boot App.

1. Create the `myapp.service` file as following.
    ```
    sudo nano /etc/systemd/system/myapp.service
    ```
2. Copy following contents to newly created `myapp.service`file, save file `(Ctrl + O)` and exit the nano editor `(Ctrl + X)`.
    ```
    [Unit]
    Description=My Java Spring Boot App
    Documentation=https://www.sma.im
    After=network.target

    [Service]
    EnvironmentFile=/etc/myapp/environmentfile
    Type=simple
    User=myapp

    WorkingDirectory=/usr/local/bin
    ExecStart=/usr/bin/java $JAVA_OPTS  -jar $EXEC_JAR

    StandardOutput=journal
    StandardError=journal
    SyslogIdentifier=myapp

    SuccessExitStatus=143
    TimeoutStopSec=10
    Restart=on-failure
    RestartSec=60

    [Install]
    WantedBy=multi-user.target    
    ```

## Load service and configure it to start on boot

Run the `systemctl daemon-reload` command to reload systemd manager configuration, this is soft reload otherwise we will need to restart the system to get our newly cerated *systemd service* recognized by *systemd*.
    ```
    sudo systemctl daemon-reload
    ```

We can use `systemctl enable` command to enable our service for auto start on system boot.

    ```
    sudo systemctl enable myapp.service
    ```

## Staring and Stopping our Systemd service

We can start *myapp* service using `service` command as following.

```
sudo service myapp start
```

Run following command to stop *myapp* service.

```
sudo service myapp stop
```

## Check current status of myapp service

We can check current status of *myapp* service using `status` option of `service` command.

```
sudo service myapp status
```

## Viewing logs of our Java Spring Boot App

 `StandardOutput=journal` and `StandardError=journal`options in our *service unit file* will enable us to view logs emitted by our program `myapp` using `journalctl` command as following.

```
journalctl -u myapp.service 
```


That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
