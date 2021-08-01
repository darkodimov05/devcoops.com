---
layout: post
title:  "How to install PostgreSQL on Rocky Linux 8"
categories: [ postgresql ]
image: assets/images/rocky-postgre.jpg
tags: [ postgresql, psql ]
---
Installing `PostgreSQL` on Rocky Linux follows the same procedure as MySQL. We need to enable the PostgreSQL module, install the desired version and enable the PostgreSQL service. Follow the next few steps and you will learn how to install PostgreSQL on Rocky Linux.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install PostgreSQL on Rocky Linux 8
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**. List all the active PostgreSQL versions:
```bash
sudo dnf module list postgresql
```
`Output:`
```bash
Rocky Linux 8 - AppStream
Name                                         Stream                                  Profiles                                            Summary                                                             
postgresql                                   9.6                                     client, server [d]                                  PostgreSQL server and client module                                 
postgresql                                   10 [d]                                 client, server [d]                                  PostgreSQL server and client module                                 
postgresql                                   12                                      client, server [d]                                  PostgreSQL server and client module                                 
postgresql                                   13                                      client, server [d]                                  PostgreSQL server and client 
module 
```

**Step 3**. We can see that the default version is `10`. Disable it with the following command:
```bash
sudo dnf -qy module disable postgresql
```

**Step 4**. To install the latest version add the official repo:
```bash
sudo dnf -y install https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

**Step 5**. Install the latest PostgreSQL version:
```bash
sudo dnf -y install postgresql13 postgresql13-server
```

**Step 6**. Initialize the service:
```bash
sudo /usr/pgsql-13/bin/postgresql-13-setup initdb
```
`Output:`
```bash
Initializing database ... OK
```

**Step 7**. Start and enable the service:
```bash
sudo systemctl enable --now postgresql-13
sudo systemctl start postgresql-13
sudo systemctl status postgresql-13
```

## Conclusion
This tutorial shows you how to install the latest PostgreSQL version on Rocky Linux 8.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.