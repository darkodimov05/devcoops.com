---
layout: post
title:  "Show Database and table size in PostgreSQL"
categories: [ postgresql ]
image: assets/images/postgresql-db-table-size.jpg
tags: [ postgresql, psql ]
---
In this tutorial, we'll see how we can show a database and table size in PostgreSQL. Planning a backup procedure, determing server instance size, could be few reasons to keep an eye on it.

## Prerequisites
* PostgreSQL

## Get database size
**Step 1**. Login to the postgresql server:
```bash
psql -h <server_name> -U <username> -W
```

**Step 2**. Once prompt for password, enter the password and type the following command to determine the db size:
```bash
SELECT pg_size_pretty(pg_database_size('<db_name>'));
```

**Step 3**. Show size of all databases in DESC order:
```bash
SELECT pg_database.datname as "db_name", pg_database_size(pg_database.datname)/1024/1024 AS size_in_mb FROM pg_database ORDER by size_in_mb DESC;
```

## Get table size
**Step 1**. Login to the postgresql server:
```bash
psql -h <server_name> -U <username> -W 
```

**Step 2**. Get table size:
```bash
SELECT pg_size_pretty(pg_total_relation_size('<table_name>'));
```

**Step 3**. Show size of all tables in DESC order:
```bash
select table_name, pg_relation_size(quote_ident(table_name))/1024/1024 AS size_in_mb
from information_schema.tables
where table_schema = 'public'
order by size_in_mb DESC
```

## Conclusion
Speaking of DevOps, as a good practice plan, choose and deploy a monitoring tool that will cover the steps above. In most of the time, you don't want to login to a database instance just to list few things, except maybe for troubleshooting purposes.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.