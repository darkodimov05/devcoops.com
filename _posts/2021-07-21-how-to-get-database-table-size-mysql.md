---
layout: post
title:  "How to get database and table size in MySQL"
categories: [ mysql ]
image: assets/images/mysql-db-table-size.jpg
tags: [ mysql ]
---
In this tutorial, we'll see how we can show a single and multiple database and table size in MySQL. Let's get straight to the point.

## Prerequisites
* MySQL

## Get database size
**Step 1**. Login to the mysql server:
```bash
mysql -h <server_name> -u <username> -p
```

**Step 2**. Once prompt for password, enter the password and type the following command to determine a single database size:
```bash
SELECT
    table_schema AS 'db_name',
    ROUND((data_length + index_length) / 1024 / 1024) AS 'size_in_mb'
FROM
    information_schema.tables
WHERE
    table_schema = '<db_name>'
GROUP BY
    table_schema;
```

**Note**: Make sure to replace `<db_name>` with the actual database name in the `WHERE` claus.

**Step 3**. Size of all databases in DESC order:
```bash
SELECT
    table_schema AS 'db_name',
    ROUND((data_length + index_length) / 1024 / 1024) AS 'size_in_mb'
FROM
    information_schema.tables
GROUP BY
    table_schema;
ORDER BY
    (data_length + index_length)
    DESC;
```

## Get table size
**Step 1**. Login to the mysql server:
```bash
mysql -h <server_name> -u <username> -p
```

**Step 2**. Size of the specific table:
```bash
SELECT
    table_name AS 'table_name',
    ROUND((data_length + index_length) / 1024 / 1024) AS 'size_in_mb'
FROM
    information_schema.TABLES
WHERE
    table_schema = '<db_name>'
    AND table_name = '<table_name>'
```

**Note**: Before running the SQL query, make sure to replace `<db_name>` and `<table_name>` with the actual database and table names.

**Step 3**. Size of all tables in DESC order:
```bash
SELECT
    table_name AS 'table_name',
    ROUND((data_length + index_length) / 1024 / 1024) AS 'size_in_mb'
FROM
    information_schema.TABLES
WHERE
    table_schema = '<db_name>'
ORDER BY
    (data_length + index_length)
    DESC;
```

**Note**: Before running the SQL query, make sure to replace `<db_name>` with the actual database name.

## Conclusion
My 2 cents are to deploy proper monitoring and alerting tools that will keep an eye on the database and table size growth.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.