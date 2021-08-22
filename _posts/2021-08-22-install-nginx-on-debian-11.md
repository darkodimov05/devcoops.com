---
layout: post
title:  "How to install Nginx on Debian 11"
categories: [ nginx ]
image: assets/images/debian11-nginx.jpg
tags: [ nginx, debian, debian 11 ]
---
Installing Nginx on Debian 11 is pretty easy and straightforward. This tutorial will show you how to install it and enable HTTP traffic on your debian server.

## Prerequisites
* Debian 11

## Install latest Nginx version on Debian 11
**Step 1**. Update the packages:
```bash
sudo apt update
```

**Step 2**. Install `nginx`:
```bash
sudo apt install nginx
```

**Step 3**. Verify the installation:
```bash
nginx -v
```
`Output:`
```bash
nginx version: nginx/1.18.0
```

## Enable HTTP traffic
**Step 1**. List the application profiles:
```bash
sudo ufw app list
```
`Output:`
```bash
Available applications:
  ...
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  ...
```

**Step 2**. Since we haven't configured SSL yet, we will only allow HTTP traffic:
```bash
sudo ufw allow 'Nginx HTTP'
```

**Step 3**. Verify the change by executing:
```bash
sudo ufw status
```
`Output:`
```bash
Status: active

To                         Action      From
--                         ------      ----
...                
Nginx HTTP                 ALLOW       Anywhere
...
```

## Conclusion
As we can see from the installation section, Nginx is available in Debian default repositories which simplifies the installation process. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.
