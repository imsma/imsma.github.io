---
layout: post
title:  "Install and configure Samba on Ubuntu Server 16.04"
author: sma
categories: [ Samba, Ubuntu ]
image: assets/images/sharing-1068523.jpg
---

[Samba](https://www.samba.org) file server is implementation of [SMB (Server Message Block)](https://en.wikipedia.org/wiki/Server_Message_Block) networking protocol. **Samba** file server allows you to share your files with users running **Windows** and **macOS**.

This step by step tutorial will help you install and configure **Samba** on **Ubuntu OS**.

## Install Samba

Update apt repositories before initiating the install process.

```
sudo apt update
```

To install samba run following commands on terminal.

```
sudo apt install samba
```

### Verify Samba Installation
Run following command to verify installation of **Samba** file server.

```
whereis samba
``` 

Above command will output following text if the installation was successful.

```
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man8/samba.8.gz /usr/share/man/man7/samba.7.gz
```

## Configure Samba file sharing
Create a directory that you want to share with other.

```
mkdir ~/myshare/
```

Above command will create a directory named  `myshare` in home directory of currently logged in user. Next we will edit **Samba** configuration to share newly created directory `myshare`. 

Edit the **Samba** configuration file located at `/etc/samba/smb.conf`.

```
sudo nano /etc/samba/smb.conf
```

Add following lines at the bottom of opened configuration file.

```
[myshare]
    comment = My Samba Share
    path = /home/<username>/myshare
    read only = no
    browsable = yes
```

Save the file by pressing `Ctrl+O` and exit the nano editor by pressing `Ctrl+X`.

Restart the **Samba** service named `smbd` to reload the new configurations.

```
sudo service smbd restart
```

## Creating User Account to access share

**Samba** doesn't use the system account's password, we need to setup **Samba** password for our account. Run following command by replacing the `<username>` with a valid system user, it will ask for password and verify password. We will need to provide this password while accessing the **Samba Share** from remote computer.

```
sudo smbpasswd -a <username>
```

## Accessing the share from remote computer

### On macOS
1. Open finder and click on `Go > Connect to Server` menu item or press `Cmd + K` shortcut key.

![macOS connect to server / smb share]({{ site.baseurl }}/assets/images/macos-connect-server-menu.jpg)

2. Enter the `smb://<IP-ADDRESS>/myshare` in *Server Address* input box and Click `Connect` button.

![macOS connect to server dialog]({{ site.baseurl }}/assets/images/macOS-connect-server-dialog.jpg)

3. Choose `Registered User` option, enter `user name`, `password` and click `Connect` button to open the *Samba share*.

![macOS connect to Samba / smb share]({{ site.baseurl }}/assets/images/connect-smb-share.jpg)

### On Microsoft Windows

Follow this tutorial to access **Samba** share from Microsoft Windows 10 pc.

[Accessing Samba share from Microsoft Windows 10 PC](https://www.windowscentral.com/how-access-files-network-devices-using-smbv1-windows-10){:target="_blank"}


Thanks for reading, please let me know if you have any questions, issues or comments.




