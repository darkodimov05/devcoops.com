---
layout: post
title:  "How to list, remove and upgrade PostgreSQL cluster"
categories: [ postgresql ]
image: assets/images/postgresql-cluster-ops.jpg
tags: [ postgresql ]
---
As part of the [PostgreSQL](https://devcoops.com/categories/#postgresql){:target="_blank"} series, in today's tutorial, we are going to see on how can we list, remove and upgrade clusters, the easy way.

## Prerequisites
* PostgreSQL

## Solution
**Step 1**. First things first, let's see how many clusters we got installed on our machine:  
```bash
pg_lsclusters
```

Sample output:
```bash
Ver Cluster Port Status Owner    Data directory              Log file
10  main    5432 online postgres /var/lib/postgresql/10/main /var/log/postgresql/postgresql-10-main.log
11  main    5433 online postgres /var/lib/postgresql/11/main /var/log/postgresql/postgresql-11-main.log
```

As we can see, we got two clusters, `10` and `11`.

**Step 2**. Let's say we don't need cluster `11`. Stop and remove PostgreSQL `11` cluster:  
```bash
pg_dropcluster 11 main --stop
```

**Note**: If you want only to stop the cluster, use the command: `pg_ctlcluster stop 11 main`. `pg_ctlcluster` command supports `start/restart/reload` operations as well.

**Step 4**. Now, if we want to do an upgrade, always do a backup first.
```bash
export BCKP_DATE="$(date +%Y_%m_%d-%H:%M)"
pg_dumpall -f <backup_dir>/postgres_10_${BCKP_DATE}.sql
```

**Step 5**. Upgrade cluster `10` to `11`:  
```bash
pg_upgradecluster 10 main
```

**Step 6**. Cluster `10` should be down, so verify it:  
```bash
Ver Cluster Port Status Owner    Data directory              Log file
10  main    5432 down   postgres /var/lib/postgresql/10/main /var/log/postgresql/postgresql-10-main.log
11  main    5433 online postgres /var/lib/postgresql/11/main /var/log/postgresql/postgresql-11-main.log
```

**Step 7**. Now, we have a fresh new PostgreSQL cluster `11`. Remove any postgresql `10` packages and package dependencies:  
```bash
sudo apt-get purge postgresql-10 postgresql-client-10
```

**Note**: You can list all package dependendies using the command: `dpkg --list | grep postgresql-10`.

## Conclusion
Always do backups before doing major PostgreSQL upgrades.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.