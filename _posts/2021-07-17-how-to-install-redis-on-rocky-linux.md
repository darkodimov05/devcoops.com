---
layout: post
title:  "How to install Redis on Rocky Linux 8"
categories: [ redis ]
image: assets/images/redis-rocky.jpg
tags: [ redis, rockylinux, linux, cli ]
---
Redis stands as an open-source in-memory key-value data store and you can use it as a database, cache, message broker, and queue. Redis has also been ranked as the `#4 NoSQL` database in user satisfaction and market presence based on user reviews. Here we will show you how to install Redis on Rocky Linux which is a replacement for CentOS.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install Redis on Rocky Linux 8
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**. Enable `EPEL` repository :
```bash
sudo dnf install epel-release
```

**Step 3**. Install Redis:
```bash
sudo dnf install redis
```

**Step 4**. Enable and start the Redis service:
```bash
systemctl start redis
systemctl enable redis
systemctl status redis
```

**Step 5**. To login to the Redis shell and ping the redis server run:
```bash
redis-cli
```
`Output:`
```bash
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

## Uninstall Redis on Rocky Linux
**Step 1**. If you decide to remove redis on Rocky Linux execute:
```bash
sudo dnf remove redis
```

## Conclusion
This tutorial shows you how to install the latest Redis version on Rocky Linux 8, and if you decide to use another caching tool instead of Redis there is a section on how to remove it depending on your needs.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.