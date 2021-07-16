---
layout: post
title:  "How to refresh AWS ECR authorization token"
categories: [ aws ]
image: assets/images/aws-ecr-auth-token.jpg
tags: [ aws, ecr, cli, docker ]
---
AWS ECR is a managed container registry which allows us to store, manage and deploy container images, mostly Docker images. Similar to Docker Hub, ECR supports two types of registries: public and private. As you may guess, you will need some kind of authentication and authorization in order to pull or push images on private registry. At the time of writing there are three methods for private authentication:

* Amazon ECR credential helper
* Authorization token
* HTTP API authentication

In this post I'll cover the authorization token method.

## Prerequisites
* AWS CLI
* Docker

## Refresh ECR authorization token manually
There are two ways to authenticate Docker to an AWS ECR private registry, using `aws ecr get-login-password` and `aws ecr get-login` commands. Let's go with the first one since it's a little bit more secure because it doesn't expose the password from the CLI.

**Step 1**. Make sure that AWS CLI is installed and configured on your local machine.

**Step 2**. Run the command manually.
```bash
aws ecr get-login-password --region <region_here> | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
```  
`Output:`
```bash
Login Succeeded
```

## Automate the ECR authorization token refresh
**Step 3**. Since AWS ECR authorization token lasts for 12 hours we can create a simple cron job by running the command `crontab -e` and adding the following line:
```bash
0 */12 * * * aws ecr get-login-password --region <region_here> | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
```
**Note**: CI tools like Jenkins provides a way to add cron jobs as well, but at the end of the day, applying the Authorization token method on servers it's a bad idea from security standpoint.

## Conclusion
Choosing the Authorization token method over the credential helper it's a bad idea in terms of security, because it's exposing the password unencrypted in your docker config.json file, but on the other hand, I find it more handy then using HTTP API authentication with `curl` or some other API management tool. To sum it up, If you are working on your local machine and you need to pull or push some images once in a while, the Authorization token method could do just fine, since it doesn't require installing additional software (credential helper). But, if an EC2 instance (Jenkins CI, ECS or EKS worker node) needs access for pulling and deploying images from ECR private registries then creating and attaching an IAM Policy to an EC2 Instance Profile should be the best way to do it.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.