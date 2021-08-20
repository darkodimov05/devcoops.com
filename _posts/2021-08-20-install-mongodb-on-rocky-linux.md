---
layout: post
title:  "How to install MongoDB on Rocky Linux 8"
categories: [ mongodb ]
image: assets/images/rocky-mongodb.jpg
tags: [ mongodb, cloud, rocky, linux, rockylinux ]
---
MongoDB is a popular NoSQL database. It offers faster query processing but with an increased load and system requirements, creating Binary JSON format (BSON) to increase efficiency and support more data types. Data stored in BSON can be searched and indexed, tremendously increasing performance. Installing mongodb on rocky linux can additionally improve the search performances cause rocky linux is well known as a minimalistic distro.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install MongoDB on Rocky Linux 8
**Step 1**. First create the mongodb repository:
```bash
sudo nano /etc/yum.repos.d/mongodb-org.repo
```

**Step 2**. Copy and paste the following config below, to enable the latest mongodb version:
```bash
[mongodb-org-5.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/5.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc
```

**Step 3**. Update the system packages to sync with the newly added:
```bash
sudo dnf update
```

**Step 4**. Install the latest version of mongodb:
```bash
sudo dnf install mongodb-org
```

**Step 5**. Verify the installation:
```bash
mongod --version
```
`Output:`
```bash
db version v5.0.2
Build Info: {
    "version": "5.0.2",
    "gitVersion": "6d9ec525e78465dcecadcff99cce953d380fedc8",
    "openSSLVersion": "OpenSSL 1.1.1g FIPS  21 Apr 2020",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "rhel80",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```

## Start and Enable MongoDB Service 
```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```
To check the status execute:
```bash
sudo systemctl status mongod
```

## Conclusion
As I mentioned previously installing mongodb on rocky linux can additionally improve the search performances, so don't hesitate to install MongoDB on your new, fresh rocky linux instance. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.