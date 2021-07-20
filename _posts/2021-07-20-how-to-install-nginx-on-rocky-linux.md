---
layout: post
title:  "How to install Nginx on Rocky Linux 8"
categories: [ nginx ]
image: assets/images/nginx-rocky.jpg
tags: [ nginx, rockylinux, linux ]
---
If you need to implement a reverse proxy, load balancer or mail proxy, Nginx is the best-suited web server that can handle all of these actions. This tutorial will explain, how to install it on `Rocky Linux 8` in the next few steps:

## Prerequisites
* Rocky Linux 8
* sudo access

## Install Nginx on Rocky Linux 8
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**. Install `Nginx`:
```bash
sudo dnf install nginx
```

**Step 3**. After installing it you can check the `nginx` version with the following command:
```bash
sudo nginx -v
```
`Output:`
```bash
nginx version: nginx/1.14.1
```

**Step 4**. Now enable and start the `nginx` service:
```bash
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```

## Uninstall Nginx on Rocky Linux 8
**Step 1**. If you decide to use another web server rather than `nginx` you  can remove it with the following command:
```bash
sudo dnf remove nginx
```

## Conclusion
This tutorial shows you how to install the latest `Nginx` version on Rocky Linux 8, and if you decide to use another web server instead of `Nginx` there is a section on how to remove it, depending on your needs.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.