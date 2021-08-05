---
layout: post
title:  "How to terminate connections in PostgreSQL"
categories: [ postgresql ]
image: assets/images/postgresql-terminate-connections.jpg
tags: [ postgresql ]
---
As part of the [PostgreSQL](https://devcoops.com/categories/#postgresql){:target="_blank"} series, in today's tutorial, we are going to focus on how to list and terminate connections. `terminate`, `kill` and `drop` will be used interchangeably through the post.

## Prerequisites
* PostgreSQL

## Drop all connections to a specific database except yours
```bash
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE pg_stat_activity.datname = '<database_name>'
  AND pid <> pg_backend_pid();
```

## List and kill idle connections
**Step 1**. First things first, list all idle connections:
```bash
SELECT pid, usename, datid, datname, state FROM pg_stat_activity WHERE state = 'idle';
```

**Step 2**. Kill an idle connection:
```bash
SELECT pg_terminate_backend(<pid>);
```

## Conclusion
Terminating connections manually is not a good practice, but it works good in development and testing environments. Always make sure to find the root cause why you have idle, or unwanted connections in the first place.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.