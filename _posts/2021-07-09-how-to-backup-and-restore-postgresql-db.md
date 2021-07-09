---
layout: post
title:  "How to Backup and Restore a PostgreSQL DB"
categories: [ postgresql ]
image: assets/images/postgresql-bkp-restore.jpg
tags: [ postgresql, pg_dump, pg_restore, psql ]
---
As an open-source database, PostgreSQL is in the top 3 most popularly used databases which offen can be find as a managed service 
offered by the public cloud giants. In one of the previous posts, I've wrote about how easily you can create a PostgreSQL database
in Azure. You can find the link [here]({% post_url 2019-10-23-how-to-create-azure-postgresql-database-using-azure-cli %}){:target="_blank"}.  
Anyway, in today's blog post, I'm going to show you how can you backup and restore postgreSQL databases from the command line.
Backing up and restoring databases could be found quite handy, especially in situations when something goes wrong. The most common cases are:
someone accidently drop the database, rollbacking a new release, or even worse, unauthorized access of your company data.  
 **Note**: `backup` and `dump` will be used interchangeably through the post.

## Prerequisites
* PostgreSQL

## Backing up a PostgreSQL database
Backing up a postgreSQL can be done in multiple ways, from the cli using the `pg_dump` utility, or via GUI using `pgAdmin`. Let's see how can we do a backup via `pg_dump`:
```bash
pg_dump -h 0.0.0.0 -p 5432 -U <username> -Fc <db_name> > <db_name>.dump
```
---
**NOTE**

* `-h` hostname
* `-p` port
* `-U` username
* `-Fc` custom format for the output file

Make sure the postgreSQL username has enough permissions to execute a backup.
If you are not exposing the PostgreSQL server to any address which you shouldn't, replace `0.0.0.0` with `localhost`.

---

For larger size db dumps, you can compress it as well. For example:
```bash
pg_dump -h 0.0.0.0 -p 5432 -U <username> -d <db_name> | gzip > <db_name>.dump.gz
```

## Restoring a PostgreSQL database
Restoring a database can be done in multiple ways as well, from the cli using `psql`, `pg_restore` utilities, or via GUI using `pgAdmin`. Let's see how can we do it from the command line.  
`pg_restore`:
```bash
pg_restore -h 0.0.0.0 -p 5432 -U <username> -d <db_name> <db_name>.dump
```

`psql`:
```bash
psql -h 0.0.0.0 -p 5432 -U <username> <db_name> < <db_name>.dump
```

Restore compressed backup using `pg_restore`:
```bash
gunzip -c <db_name>.dump.gz | pg_restore -h 0.0.0.0 -p 5432 -U <username> -d <db_name>
```

Restore compressed backup using `psql`:
```bash
gunzip -c <db_name>.dump.gz | psql -h 0.0.0.0 -p 5432 -U <username> <db_name>
```

## Conclusion
My 2 cents are always have a backup and recovery procedures for your databases, or even your infrastructure. Review, update and automate it frequently. An early adoption could save a lot of time and stress, especially if things go south on the weekends.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.