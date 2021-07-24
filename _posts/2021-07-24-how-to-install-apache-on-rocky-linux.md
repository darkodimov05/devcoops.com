---
layout: post
title:  "How to install Apache on Rocky Linux 8"
categories: [ apache ]
image: assets/images/apache-rocky.jpg
tags: [ apache, rockylinux, linux, cli ]
---
Apache is one of the most popular, free, and open source web servers that allows you to deliver web content through the internet. Apache is a highly reliable web server and it has great performance. Apache can be installed easily and can run on almost any operating system like Windows, Linux, etc. In this tutorial, we are going to install it on the latest version of `Rocky Linux`.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install Apache on Rocky Linux 8
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**. Install `Apache`:
```bash
sudo dnf install httpd
```

**Step 3**. Once it is installed start and enable the service:
```bash
systemctl start httpd
systemctl enable httpd
systemctl status httpd
```
`Output:`
```bash
httpd.service - The Apache HTTP Server
    Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
    Active: active (running)
```

**Step 4**. If you want to check the version run:
```bash
httpd -v
```
`Output:`
```bash
Server version: Apache/2.4.37 (rocky)
```

**Step 5**. If `firewalld` is enabled, make sure to allow `http` and `https` protocols:
```bash
sudo firewall-cmd --permanent --add-service={http,https}
sudo firewall-cmd --reload
```

## Uninstall Apache on Rocky Linux
If you decide to remove `Apache` on Rocky Linux execute:
```bash
sudo dnf remove httpd
```

## Conclusion
This tutorial shows you how to install the latest `Apache` version on Rocky Linux 8, and if you decide to use another web server instead of `Apache` there is a section on how to remove it. 
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.