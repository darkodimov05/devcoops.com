---
layout: post
title:  "How to stop all running EC2 instances from the command line"
categories: [ aws ]
image: assets/images/aws-stop-ec2.jpg
tags: [ aws, ec2, cli ]
---
Stopping preproduction, staging environment EC2 instances could be described sometimes as a good practice,
espesially from cost and even a security standpoint. In the next few lines, I'll show you how you can easily stop 
all running EC2 instances from the cli in a single command.

## Prerequisites
* AWS CLI

## Stop all running EC2 instances
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

**Step 3**. Get a list of all running EC2 instances
```bash
aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query 'Reservations[].Instances[].InstanceId'
```
Depending on the number of instances, you will get a json response including the instances ids.  
`Output:`
```bash
[
    "i-<some-instance-id>",
    "i-<some-other-instance-id>"
]
```

**Step 4**. Replace new lines with spaces using the linux command line utility `tr`
```bash
aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query 'Reservations[].Instances[].InstanceId' --output text | tr '\n' ' '
```
`Output:`
```bash
i-<some-instance-id> i-<some-other-instance-id> %
```

**Step 5**. It's a good practice to always execute a dry run
```bash
aws ec2 stop-instances --dry-run --instance-ids $(aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query 'Reservations[].Instances[].InstanceId' --output text | tr '\n' ' ')
```
`Output:`
```bash
An error occurred (DryRunOperation) when calling the StopInstances operation: Request would have succeeded, but DryRun flag is set.
```

**Step 6**. Remove the `--dry-run` flag and stop all running EC2 instances
```bash
aws ec2 stop-instances --instance-ids $(aws ec2 describe-instances --filters Name=instance-state-name,Values=running --query 'Reservations[].Instances[].InstanceId' --output text | tr '\n' ' ')
```

## Conclusion
The next key step is automation. You could use a cronjob on your local machine, HA server or even better, deploy a lambda function that will trigger the stop command. On the other hand, don't forget to create a command that will start all stopped EC2 instances every morning. This will be covered in the next upcoming posts.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.