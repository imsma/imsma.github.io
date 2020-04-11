---
layout: post
title:  "Install Docker and Docker Compose On Ubuntu Linux Server"
author: sma
categories: [ MySQL, Linux, Ubuntu]
image: assets/images/posts/server-grid.jpg
description: "Install Docker and Docker Compose On Ubuntu Linux Server"
---

In this article I will show you how to install docker and docker compose on Ubuntu Linux Server.


## Install Docker Engine - Community for Ubuntu

This is Prerequisite


### Uninstall old versions

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Install Docker Engine - Community using repository


#### Repository setup

```
sudo apt-get update
```

??Install packages to allow apt to use a repository over HTTPS

```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

?? Add Docker’s official GPG key:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Sample output will be

```
OK
```


??
Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.

???

```
sudo apt-key fingerprint 0EBFCD88
```

Sample output will be

```
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

set up the stable repository using following commands
??
`lsb_release -cs` sub-command below returns the name of your Ubuntu distribution, such as xenial
???

```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

#### INSTALL DOCKER ENGINE - COMMUNITY

Update the apt package index.

```
sudo apt-get update
```

Sample output will be

```
Hit:1 http://ppa.launchpad.net/webupd8team/java/ubuntu xenial InRelease
Hit:2 http://us.archive.ubuntu.com/ubuntu xenial InRelease                                                                                           
Get:3 http://repo.mysql.com/apt/ubuntu xenial InRelease [21.6 kB]                             
Get:4 http://security.ubuntu.com/ubuntu xenial-security InRelease [109 kB]                              
Get:5 http://us.archive.ubuntu.com/ubuntu xenial-updates InRelease [109 kB]      
Ign:3 http://repo.mysql.com/apt/ubuntu xenial InRelease                                        
Get:6 https://download.docker.com/linux/ubuntu xenial InRelease [66.2 kB]                      
Get:7 http://us.archive.ubuntu.com/ubuntu xenial-backports InRelease [107 kB]  
Get:8 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages [12.0 kB]      
Fetched 425 kB in 2s (147 kB/s)
Reading package lists... Done
W: GPG error: http://repo.mysql.com/apt/ubuntu xenial InRelease: The following signatures were invalid: KEYEXPIRED 1550412832  KEYEXPIRED 1550412832  KEYEXPIRED 1550412832
W: The repository 'http://repo.mysql.com/apt/ubuntu xenial InRelease' is not signed.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

#### Install the latest version 

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

#### Install a specific version of Docker Engine

To install a specific version of Docker Engine


??List the versions available in your repo:

```
apt-cache madison docker-ce
```

Sample output will be 

```
docker-ce | 5:19.03.5~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:19.03.4~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:19.03.3~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:19.03.2~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:19.03.1~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:19.03.0~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:18.09.9~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:18.09.8~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:18.09.7~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:18.09.6~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:18.09.5~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 docker-ce | 5:18.09.4~3-0~ubuntu-xenial | https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
 .....
```

?? Install a specific version using the version string from the second column, for example,

```
5:19.03.3~3-0~ubuntu-xenial
```

Use following command to install specific version of docker engine

```
sudo apt-get install docker-ce=<DOCKER_VERSION> docker-ce-cli=<DOCKER_VERSION> containerd.io
```

Replace `DOCKER_VERSION` with   appropriate version string such as `5:19.03.3~3-0~ubuntu-xenial`.


#### Install Compose on Linux systems

?? Run this command to download the current stable release of Docker Compose:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

?? Apply executable permissions to the binary:

```
sudo chmod +x /usr/local/bin/docker-compose
```

??
If the command docker-compose fails after installation, check your path. You can also create a symbolic link to /usr/bin or any other directory in your path.
???

```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

Verify the isntallation using following command

```
docker-compose --version
```

Sample output

```
docker-compose version 1.25.0, build 0a186604
```










That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
