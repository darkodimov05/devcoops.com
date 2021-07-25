---
layout: post
title:  "How to join EC2 instance to ECS cluster"
categories: [ aws ]
image: assets/images/ec2-instance-ecs.jpg
tags: [ aws, ecs, ec2 ]
---
AWS ECS is a container orchestration tool and it could be a great starting point as a beginner devops engineer before jumping on the Kubernetes hype train. Using an Auto Scaling group with ECS is usually a bread and butter, but recently I was playing around with Terraform and my ECS cluster couldn't detect any container instances. Also, this could happen if you try to join an already running ECS optimized EC2 instance to a cluster. So, let's see what's the deal.

## Prerequisites
* ECS-Optimized EC2 instance
* ECS cluster

## Solution #1
By default, when you spin up an EC2 instance it will join the default cluster. If we want to register the instance in a non-default cluster, we need to pass the following script into the `User data` field.  
```bash
#!/bin/bash

echo ECS_CLUSTER=your_cluster_name >> /etc/ecs/ecs.config
```

## Solution #2
Adding the `ECS_CLUSTER` into the `ecs.config` file is just one way to do it. You could also pull the `ecs.config` file from a S3 bucket. For example:
```bash
#!/bin/bash

yum install -y aws-cli
aws s3 cp s3://your_bucket_name/ecs.config /etc/ecs/ecs.config
```

## Conclusion
If you go with solution #2, make sure the EC2 instance profile IAM role has read-only access to the S3 bucket.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.