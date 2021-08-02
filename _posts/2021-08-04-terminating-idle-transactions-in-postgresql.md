---
layout: post
title:  "How to terminate idle transactions in PostgreSQL"
categories: [ postgresql ]
image: assets/images/postgresql-idle-transactions.jpg
tags: [ postgresql ]
---
As part of the [PostgreSQL](https://devcoops.com/categories/#postgresql){:target="_blank"} series, in today's tutorial, we are going to see on how to set `idle_in_transaction_session_timeout` parameter and handle idle transactions, so you won't face any table bloats.

## Prerequisites
* PostgreSQL

## Solution
The default value for `idle_in_transaction_session_timeout` is 0, which means disabled.

**Step 1**. First things first, get the `idle_in_transaction_session_timeout` parameter value:   
```bash
SHOW idle_in_transaction_session_timeout;
```

Example output:
```bash
 idle_in_transaction_session_timeout 
-------------------------------------
 0
(1 row)
```

**Step 2**. Uncomment and set `idle_in_transaction_session_timeout` parameter value globally in the `/var/lib/postgresql/data/postgresql.conf` file. Let's set the value to 10 seconds for example:
```bash
idle_in_transaction_session_timeout = 10000        # in milliseconds, 0 is disabled
```

Or, if you are running PostgreSQL on a managed service, for example Azure, you can set the parameter using the following command:  
```bash
ALTER system SET idle_in_transaction_session_timeout='10000';
```

**Note**: The changes will apply globally, and i don't recommend it to be honest.

### Update idle_in_transaction_session_timeout for a specific database
```bash
psql -U <username> -d <database> -W
SET idle_in_transaction_session_timeout TO '5000';
```

### Update idle_in_transaction_session_timeout for a specific user
```bash
ALTER USER devcoops SET idle_in_transaction_session_timeout TO '5000';
```

## Conclusion
My 2 cents is to investigate what causes the connections to be in a idle state instead of calling it a day with a single update in the conf file.   
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.