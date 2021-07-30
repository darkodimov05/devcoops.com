---
layout: post
title:  "How to install MySQL on Rocky Linux 8"
categories: [ mysql ]
image: assets/images/rocky-mysql.jpg
tags: [ mysql, rockylinux, linux ]
---
Installing MySQL on Rocky Linux it's quite easy and quick. MySQL is an open-source relational database management system and it's used for a wide range of purposes, including data warehousing, e-commerce, and logging applications. The most common use is for the purpose of a web database.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install MySQL on Rocky Linux 8
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**.Enable the MySQL `AppStream` module:
```bash
sudo dnf module enable mysql:8.0
```

**Step 3**. Install MySQL:
```bash
sudo dnf install @mysql
```

**Step 4**. Enable and start the MySQL service:
```bash
sudo systemctl enable --now mysqld
sudo systemctl start mysqld
sudo systemctl status mysqld
```

**Step 5**. To check the version execute:
```bash
mysql --version
```

## Secure MySQL 8 on Rocky Linux 8
After installing the `MySQL` server, good practice is to secure the database server executing the following command:
```bash
sudo mysql_secure_installation
```

## Conclusion
This tutorial shows you how to install and secure the latest MySQL version on Rocky Linux 8.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.