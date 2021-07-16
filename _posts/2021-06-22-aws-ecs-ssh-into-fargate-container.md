---
layout: post
title:  "SSH into an AWS ECS Fargate managed container"
categories: [ aws ]
image: assets/images/aws-fargate.jpg
tags: [ aws, ecs, eks, fargate ]
---
AWS Fargate is a serverless compute engine for containers and works with both ECS and EKS. Fargate makes it easy for you to focus on building your applications and the idea behind that is to avoid self-managing ECS or EKS containers. Fargate allocates the right amount of compute, eliminating the need to choose instances and scale cluster capacity. You only pay for the resources required to run your containers, so you need to make sure if fargate is the right choice for you. Fargate runs each task or pod in its own kernel providing the tasks and pods their own isolated compute environment, so there wasn't an option to ssh into the container and execute some extra commands. Due to the lack of container ssh connectivity, there were a lot of discussions and complaints, so finally on 16 March 2021 AWS announced the ability for all Amazon ECS users including developers and operators to “exec” into a container running inside a task deployed on either Amazon EC2 or AWS Fargate.

## Prerequisites
* AWS account  
* AWS CLI configured

## Establish ssh connection into fargate container
**Step 1**. Before we can connect to the Fargate container, please make sure that you have installed and configured `aws cli` properly. 
If not you can find it at: [aws cli](https://devcoops.com/install-AWS-CLI-and-create-multiple-named-profiles/)

**Step 2**. To be able to connect to the Fargate container you will have to check your `aws cli` version.
```bash
aws --version
```
You need to make sure that you have at least `version 2.0.0` otherwise you will not be able to connect.

**Step 3**. If you have a version that is less than `2.0.0` you need to update it with the following command:
```bash
curl -Lo ~/.local/aws.zip https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip; unzip ~/.local/aws.zip -d ~/.local/; ~/.local/aws/./install -u -i ~/.local/aws-cli -b ~/.local/bin; rm -rf ~/.local/aws/ ~/.local/aws.zip
```

**Step 4**. For the `ECS task role` attach a policy that allows the container to open the secure channel session via SSM
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssmmessages:CreateControlChannel",
                "ssmmessages:CreateDataChannel",
                "ssmmessages:OpenControlChannel",
                "ssmmessages:OpenDataChannel"
            ],
            "Resource": "*"
        }
}
```
**Step 5**. For the `ECS task execution role` attach the existing standard AWS managed policy `AmazonECSTaskExecutionRolePolicy`.

**Step 6**. Now we can ssh into the container using the following command:
```bash
aws ecs execute-command --region {name-of-the-region} --cluster {name-of-the-cluster} --task {task number} --container {container-name} --command "/bin/bash" --interactive
```
After executing the command you will be connected inside the container, and you can make the needed changes.

## Conclusion
This tutorial shows you how can you connect your AWS ECS or EKS Fargate container trough ssh. For more info visit the [aws documentation](https://aws.amazon.com/blogs/containers/new-using-amazon-ecs-exec-access-your-containers-fargate-ec2/).     
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.