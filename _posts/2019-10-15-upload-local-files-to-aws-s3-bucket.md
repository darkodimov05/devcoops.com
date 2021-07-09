---
layout: post
title:  "How to upload your local files to your AWS S3 bucket with AWS CLI"
categories: [ aws ]
image: assets/images/mybucket.jpg
tags: [ aws, cloud, aws services, linux ]
---
There are a lot of reasons for moving your local files into your AWS S3 bucket. Maybe you want to host your static files with S3 or you want to make a backup of your database, etc... In this tutorial we will show how to do this in a quck and efficient way with AWS CLI, whithout wasting your time by clicking on your AWS Console Management buttons. Using AWS CLI will speed up your uploading process and also you can include a lot of options with only one command. 

## Prerequisites
* You should already have an existing AWS account
* Make sure that you are logged into your Linux machine as a user with `sudo` privileges
* Have installed [AWS CLI and learned how to switch between multiple named profiles](https://devcoops.com/install-AWS-CLI-and-create-multiple-named-profiles/)

## Examples
**Step 1**. If you meet all the above requirements, it's a good practise to list all your available buckets, so you should be able to find your desired S3 bucket.
To do a simple list command run the following:
```bash
aws s3 ls
```
`Output`:
```bash
2019-10-03 18:58:59 cf-templates-zdqxzzv9nyis-us-east-1
2019-10-08 15:19:05 devcoops-bucket
```
In this tutorial our desired `bucket` will be `devcoops-bucket`. So, if you want to list all the objects inside your desired bucket run:
```bash
aws s3 ls s3://devcoops-bucket
```
This will list all the objects in the `devcoops-bucket`. If you want to see the files in a previously listed object you should use the same command and put `/name-of-the-object`.

You can also use the following command to recursively list objects in a bucket rather than playing with the bucket path:
```bash
aws s3 ls s3://devcoops-bucket --recursive
```
If you want to display file size, the total nubmer of objects and calculate the total size run:
```bash
aws s3 ls s3://devcoops-bucket --recursive --human-readable --summarize
```
`Output`:
```bash
2019-10-03 18:58:59 10 Bytes des.txt
2019-10-08 15:19:05 23 Bytes bes.txt

Total Objects: 2
   Total Size: 33 Bytes
```

**Step 2**. The general syntax of copying files goes:
```bash
aws s3 cp source destination
```
To copy from your local Linux machine to S3:

`aws s3 cp [destination-of-the-local-file] S3BuckerURI`

and the opposite logic:

`aws s3 cp S3BuckerURI [destination-of-the-local-file]`

You can also copy from one S3 bucket to another:

`aws s3 cp S3BuckerURI1 S3BuckerURI2`

To copy a single local file from your machine to S3:
```bash
aws s3 cp test.txt s3://devcoops-bucket/
```
To copy all your files in a specific folder to S3 into the desired `data` directory located in the `devcoops-bucket` bucket:
```bash
aws s3 cp my-data/ s3://devcoops-bucket/data/ --recursive
```
We can add more complex commands just in case if you want to filter your files:
In this example we will download all the files from an S3 bucket, that are starting with `2019-10-03`, but you can edit the filter by your needs:
 ```bash
aws s3 cp s3://devcoops-bucket/ my-data/ --exclude "*" --include "2019-10-03*" --recursive
```
If your goal is to synchronize a set of files without copying them twice, use the `sync` command:
```bash
aws s3 sync s3://devcoops-bucket/ my-data/
```
The following command is equivalent of the above `cp` command:
```bash
aws s3 sync s3://devcoops-bucket/ my-data/ --exclude "*" --include "2019-10-03*" --recursive
```
The difference between `cp` and `sync` commands is that, if you want to copy multiple files with `cp` you must include the `--recursive` parameter. The aws s3 `sync` command will do this by default, copy a whole directory. It will only copy new/modified files.

## Conclusion
In this tutorial we shown you how you can copy your files from and to your AWS S3 bucket. Also we shown you how to add your custom filters to copy some specific files. For more informations you can take a look at the [AWS CP Documentation](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html).