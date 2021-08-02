---
layout: post
title:  "How to resolve PostgreSQL FATAL: sorry, too many clients already"
categories: [ postgresql ]
image: assets/images/postgresql-too-many-clients.jpg
tags: [ postgresql ]
---
As part of the [PostgreSQL](https://devcoops.com/categories/#postgresql){:target="_blank"} series, in today's tutorial, we are going to see on how to deal with one of the most often seen postgresql errors:  
```bash
org.postgresql.util.PSQLException: FATAL: sorry, too many clients already.
```

The error it's pretty much self explanatory, and this is only the symptom. There is a default db connection limit of `100`, and it's seems like it's full.

## Prerequisites
* PostgreSQL

## Possible solutions
1. Increase the `max_connections` parameter. This is the most easiest one, and probably will work as a temporary solution until you figure it out the rest of the following solutions.
2. There could be a leakage, the client connections aren't properly closed on the backend side, so find out why is that.
3. Use a connection pooler.

Let's focus on solution #1, so you can buy some time preparing for the other two solutions.

**Step 1**. First things first, check the `max_connections` parameter value:   
```bash
SHOW max_connections;
```

**Step 2**. List the number of connections currently used:  
```bash
SELECT count(*) FROM pg_stat_activity;
```

**Step 3**. You can also take a look more into details regarding the current connections:  
```bash
SELECT * FROM pg_stat_activity;
```

**Step 4**. Open the postgres config file `/var/lib/postgresql/data/postgresql.conf` and update the `max_connections` parameter.
```bash
max_connections = 100
```

**Note**: Increasing the `max_connections` parameter only, is a bad idea, you need to update `shared_buffers` as well. It determines how much memory will be used by PostgreSQL for caching data. By default, the `shared_buffers` value should be 25% of the total memory in the server. With that being said,

**Step 5**. Let's say we got a server with 8GB memory, and in the same `postgresql.conf` file, update the `shared_buffers` parameter value to 2GB.
```bash
shared_buffers = 2GB
```

**Step 6**. Restart the PostgreSQL service
```bash
systemctl restart postgresql
```

## Conclusion
Not a huge fan of fixing symptoms, and increasing `max_connections` it's not always a good solution, but it'll buy you some time, especially if you dealing with this one on Friday.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.