---
layout: post
title:  "How to fix Docker login unknown shorthand flag error"
categories: [ docker, aws ]
image: assets/images/docker-aws-ecr-issue.jpg
tags: [ docker, aws, ecr, cli ]
---
In some of the previous posts I've covered the Authorization token method when authenticating to an AWS ECR private registry. If you have followed the steps as described in the [post]({% post_url 2021-07-01-how-to-refresh-ecr-authorization %}){:target="_blank"} you won't get any errors unless you try to go with `aws ecr get-login` command instead of `aws ecr get-login-password`. So, if you try to run the output from `aws ecr get-login`, you'll get the following error:
```bash
unknown shorthand flag: 'e' in -e
See 'docker login --help'.
```

## Prerequisites
* AWS CLI
* Docker

## Solution
**Step 1**. The updated command is basically the same as `aws ecr get-login` output, but without the -e flag. Something like:  
```bash
docker login -u AWS -p <some_secret_string> https://aws_account_id.dkr.ecr.region.amazonaws.com
```  
`Output:`
```bash
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

## Conclusion
The reason for seeing this error is that in 17.0.6 Docker engine update, the -e and \-\-email flags were deprecated. Check it out for yourself [Deprecated Engine Features](https://docs.docker.com/engine/deprecated/#-e-and---email-flags-on-docker-login){:target="_blank"}.  
Back in 2017, AWS came up with an announcement as well [Announcement: Docker Email Flag Removal](https://forums.aws.amazon.com/ann.jspa?annID=4656){:target="_blank"}.   
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.