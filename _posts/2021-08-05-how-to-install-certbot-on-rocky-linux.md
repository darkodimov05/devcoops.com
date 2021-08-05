---
layout: post
title:  "How to install Certbot on Rocky Linux 8"
categories: [ certbot ]
image: assets/images/certbot-rocky.jpg
tags: [ certbot, cloud, rocky, linux, rockylinux ]
---
Installing certbot on your rocky linux server/machine is usually meant to be used to switch an existing HTTP site to work in HTTPS. It's a helpful tool written in python which helps you to obtain a free Let’s Encrypt certificate depending on your web server. This tutorial will only cover the steps to install cerbot on rocky linux.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install Certbot on Rocky Linux 8
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**. Add the EPEL repository:
```bash
sudo dnf install epel-release
```

**Step 3**. Install Certbot and the required packages depending on your web server.
* If you are using `Apache` run:
```bash
sudo dnf install certbot python3-certbot-apache mod_ssl
```
* If you are using `Nginx` run:
```bash
sudo dnf install certbot python3-certbot-nginx
```

**Step 4**. To verify the certbot installation run:
```bash
certbot --version
```
You should get the latest stable version.
`Output:`
```bash
certbot 1.14.0
```

## Conclusion
In this tutorial, I covered only the steps needed to install certbot on Rocky Linux 8. In some of the next tutorials, I will cover how to obtain a free Let’s Encrypt certificate depending on your web server. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.