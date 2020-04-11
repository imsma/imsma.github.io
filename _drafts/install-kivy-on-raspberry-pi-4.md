---
layout: post
title:  "Install Kivy on Raspberry Pi 4 with Raspbian 4.19 Buster"
author: sma
categories: [ RaspberryPi,Raspbian,IoT,Python, Python3,Kivy ]
image: assets/images/posts/art-board-game-challenge-163064.jpg
description: "How to install Kivy on Raspberry Pi 4 with Raspbian 4.19 Buster ?"
tags: [featured]
---

**Kivy** is an open source **Python** library to develop cross platform applications with multitouch support and natural user interface.

Using **Kivy** we can develop apps for following platforms.

* MacOS (OS X)
* Linux (All major distributions are covered)
* Microsoft Windows OS
* iOS (iPhone and iPad)
* Android OS
* RaspberryPi (with or without touch display)
* Any touch enabled device with support for **T**angible **U**ser **I**nterface **O**bjects.

In this article we will explore the installation on Kivy on Raspberry Pi 4 with Raspbian 4.19 Buster, let's start.

## Update and Upgrade apt packages

Run `apt update` and `apt upgrade` commands as following to update the list of available packages and install available updates.

```
sudo apt update
```

```
sudo apt upgrade
```

## Install the dependencies 

Before installing **Kivy** we need following development dependencies available on our system, let's install and understanding the high level purpose of each dependency.

### **S**imple **D**irectMedia **L**ayer Development (libsdl2-dev)

*SDL* (Simple DirectMedia Layer Development) is used to develop software applications with low level access to audio, video framebuffer, keyboard and mouse. Run following command to install `libsdl2-dev` library.

```
sudo apt install libsdl2-dev
```

### Simple DirectMedia Image loading library (libsdl2-image-dev)

Simple DirectMedia Image loading library (libsdl2-image-dev) is used to load different image formats as SDL surfaces, supported image formats include PNG, PCX, LBM, WEBP, TGA, TIFF, GIF, JPEG, BPM etc. Run following command to install `libsdl2-image-dev` library.

```
sudo apt install ibsdl2-image-dev
```

### (libsdl2-mixer-dev)

```
sudo apt install libsdl2-mixer-dev
```

### (libsdl2-ttf-dev)

```
sudo apt install libsdl2-ttf-dev
```

### (pkg-config)

```
sudo apt install pkg-config
```

### (libgl1-mesa-dev)

```
sudo apt install libgl1-mesa-dev
```

### (libgles2-mesa-dev)

```
sudo apt install libgles2-mesa-dev
```


### Installing all of the development dependencies using single command

You can install all of the above development dependencies using following single command.

```
sudo apt install libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev pkg-config libgl1-mesa-dev libgles2-mesa-dev python3-setuptools libgstreamer1.0-dev git-core gstreamer1.0-plugins-{bad,base,good,ugly} gstreamer1.0-{omx,alsa} python3-dev libmtdev-dev xclip xsel libjpeg-dev
```





python3-setuptools libgstreamer1.0-dev git-core gstreamer1.0-plugins-{bad,base,good,ugly} gstreamer1.0-{omx,alsa} python3-dev libmtdev-dev xclip xsel libjpeg-dev
python3 -m pip install --upgrade --user pip setuptools
python3 -m pip install --upgrade --user Cython==0.29.10 pillow
sudo pip3 install kivy









That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
