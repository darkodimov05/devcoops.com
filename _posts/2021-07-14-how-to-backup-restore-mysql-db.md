---
layout: post
title:  "How to Backup and Restore a MySQL Database"
categories: [ mysql ]
image: assets/images/mysql-bkp-restore.jpg
tags: [ mysql ]
---
MySQL is one of the most popular open-source database engines. Manually backing up and restoring databases for whatever reasons could be found quite easy to execute, and we can see how can we do it in the following steps below.  
**Note**: `backup` and `dump` will be used interchangeably through the post.

## Prerequisites
* MySQL

## Backing up a MySQL database
Backing up a MySQL can be done in multiple ways, from the cli using the `mysqldump` command utility tool, or via GUI using `phpMyAdmin`. Let's see how can we do a backup via `mysqldump`:
```bash
mysqldump -u <username> -p <db_name> > <db_name>_$(date +%Y_%m_%d-%H:%M).sql
```
---
**NOTE**

* `-u` username
* `-p` password prompt

Make sure the MySQL username has the required permissions to execute a backup.

---

Backup and compress a database dump:
```bash
mysqldump -u <username> -p <db_name> |  gzip > <db_name>_$(date +%Y_%m_%d-%H:%M).sql 
```

Backup multiple databases:
```bash
mysqldump -u <username> -p <db1_name> <db2_name> > <db1_name>_<db2_name>_$(date +%Y_%m_%d-%H:%M).sql
```

Backup all databases:
```bash
mysqldump -u <username> -p --all-databases > all_dbs_$(date +%Y_%m_%d-%H:%M).sql
```

Backup a specific table:
```bash
mysqldump -u <username> -p <db_name> <table_name> > <db_name>_<table_name>_$(date +%Y_%m_%d-%H:%M).sql
```

## Restoring a MySQL database
Restoring a database can be done in multiple ways as well, from the cli using `mysql` command utility tool, or via GUI using `phpMyAdmin`. Let's see how can we do it from the command line.  
```bash
mysql -u <user> -p <db_name> < <db_name>_$(date +%Y_%m_%d-%H:%M).sql
```

## Conclusion
As a best practice, automating the backup procedure is a must. Usually, it can be done in multiple ways although the most common one is by creating a cronjob.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.