---
layout: post
title:  "How to pg_dump to S3 directly"
categories: [ postgresql ]
image: assets/images/pg_dump-to-s3.jpg
tags: [ postgresql, pg_dump, aws, s3 ]
---
Just before we start with this tutorial, I just wanna say, don't try this on production. Streaming pg_dump backup directly to S3 bucket is a bad practice to start with, unless you are using their PostgreSQL RDS service, which by default the backup storage is S3 and it's handled by AWS. But, on the other hand, if you have PostgreSQL deployed on a VPS, or EC2 instance for example, this could be a pretty convinient way to do backups in development environments.

## Prerequisites
* PostgreSQL
* S3 bucket
* AWS CLI (optional, if not using an EC2 instance profile)

## Dump a PostgreSQL database backup to S3 directly
So, before we execute the command, we need to make sure that we got the AWS CLI configured. If by any case, you are running the database on an EC2 instance, the best way to do it, is by using an EC2 instance profile. Keep in mind to write least privilege IAM policies. With that being said, let's get straight into it.

**Step 1**. Test the S3 bucket access by writing a test file:
```bash
echo "TEST" | aws s3 cp - s3://<bucket_name>/test.txt
```

**Step 2**. Stream the database backup directly to S3 using the following command:  
```bash
pg_dump -U <username> -Fc <database_name> | aws s3 cp - s3://<bucket_name>/<database_name>.dump
```

## Conclusion
Once you add the command as a daily cronjob, make sure to have a S3 lifecycle policy in place, as part of a backup and recovery strategy.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.