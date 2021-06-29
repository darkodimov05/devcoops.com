---
layout: post
title:  "How to list S3 bucket size from the command line"
categories: [ aws ]
image: assets/images/aws-s3-bucket-size.jpg
tags: [aws, s3, cli]
---
AWS S3 as a service is pretty cheap and probably the most popular one among the rest of the cloud object storage services. S3 is most often used as a database backup storage, log storage or even serving static files.
Although there is a way to check on how much S3 storage you are paying for from the management console, it's much more faster to do it from the command line though. Let's hope straight into it.

## Prerequisites
* AWS CLI

## List S3 bucket size
**Step 1**. Open terminal and check if AWS CLI is installed
```bash
aws --version
```
`Output:`
```bash
aws-cli/2.1.39 Python/3.9.4 Darwin/20.5.0 source/x86_64 prompt/off
```

**Step 2**. Check if AWS CLI is configured
```bash
aws configure list
```
`Output:`
```bash
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************ABCD      config_file    ~/.aws/config
secret_key     ****************ABCD      config_file    ~/.aws/config
    region                us-west-2              env    AWS_DEFAULT_REGION
```

**Step 3**. Get a summary of the S3 bucket items and size
```bash
 aws s3 ls --summarize --human-readable s3://bucket-name
```  
It will list all of the bucket items including Total Objects and Total Size.

**Step 4**. Filter out the bucket list items by adding a pipe and listing the total size using the linux command-line tool `grep`
```bash
aws s3 ls --summarize --human-readable s3://bucket-name | grep "Total Size*"
```
`Example output:`
```bash
Total Size: 105.6 GiB
```

**Step 5**. We can include Total Objects as well
```bash
aws s3 ls --summarize --human-readable s3://bucket-name | tail -2
```
`Example output:`
```bash
Total Objects: 322
Total Size: 105.6 GiB
```

## Conclusion
My two cents is to always use S3 bucket lifecycle policy or S3 intelligent tiering which will help you reduce your AWS costs.
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.