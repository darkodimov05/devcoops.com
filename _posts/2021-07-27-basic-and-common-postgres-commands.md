---
layout: post
title:  "Basic and Common PostgreSQL commands"
categories: [ postgresql ]
image: assets/images/postgres-commands.jpg
tags: [ postgresql, psql ]
---
In some of the previous posts, I've written about [How to Backup and Restore a PostgreSQL Database]({% post_url 2021-07-09-how-to-backup-and-restore-postgresql-db %}){:target="_blank"} and [How To Show Database and table size in PostgreSQL]({% post_url 2021-07-19-show-database-table-size-postgresql %}){:target="_blank"}. These are pretty common, basic and practical commands too, so let's see a few more of them that could be quite handy.

## Prerequisites
* PostgreSQL

## Basic and Practical commands
### Connect to a database
```bash
psql -h <hostname> -P <port> -U <username> -d <database> -W
```

### Switch connection to another database
```bash
\c <database>
```

### List databases
```bash
\l
```

### List schemas
```bash
\dn
```
Use `\df` for functions, `\dv` for views.

### List tables
```bash
\dt
```
**Note**: Make sure you are connected to the database you want to list the tables from.

### Describe table
```bash
\d <table_name>
```

### List users and roles
```bash
\du
```

### List available commands
```bash
\?
```

### Execute previous command
```bash
\g
```

### List command history
```bash
\s
```

### Output query results to a file
```bash
postgres=#\o output.txt
postgres=# <execute_query_here>
```
**Note**: Execute `\o` again if you want to turn it off.

### Enable query execution time
```bash
postgres=#\timing
Timing is on.
postgres=# <execute_query_here>
```
**Note**: Execute `\timing` again if you want to turn it off.

### Get PostgreSQL version
```bash
SELECT version();
```

### Count the numbers of rows in a table
```bash
SELECT count(*) FROM table;
```

### Show MAX connection parameter
```bash
SHOW max_connections;
```

### Quit / Exit
```bash
\q
```

## Conclusion
These are most commonly used ones, but if you got any other to add, feel free to write them down in the comments below.  
And, if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.