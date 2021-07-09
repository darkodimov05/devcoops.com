---
layout: post
title:  "How to fix ecs cloudformation template if it's stuck in the rollback state"
categories: [ aws ]
image: assets/images/aws-ecs-cf.jpg
tags: [ aws, ecs, cloudformation ]
---
Deploying your ECS services through CloudFormation sometimes can cause odd issues. There are a lot of reasons that can cause that. For example, if there is an application error, missing some environment variables, health checks, etc... In that situation, CloudFormation default behavior is to `rollback` the template to the previous state, but sometimes the CF stack can be stuck, and after a long time can throw `UPDATE_ROLLBACK_FAILED` state. There are a lot of tutorials that explain how to fix that by simply clicking on `Continue update rollback` next `Resources to skip` -  section and selecting the resources that you want to skip, but when it comes to ECS deployment it may not work. 

## Prerequisites
* AWS Account

## Fixing the CF stack
**Step 1**. Open the ECS service, choose the cluster and find the service from the AWS ECS services tab
![Open and find the ECS service](/assets/images/screenshots/cfecs1.png)

**Step 2**. Once the service is opened click on the `Deployments` section
![Deployments Section](/assets/images/screenshots/cfecs2.png)

**Step 3**. Click on the `Update` button on the right top corner

**Step 4**. To get back the stack in a stable state, open the `Revision` drop-down list and choose some of the previous state numbers (don't choose the `latest`) and also make sure that the `Force new deployment` is enabled: 
![Get back the stack](/assets/images/screenshots/cfecs3.png)

**Step 5**. You can leave the other configuration as it is, so that way it will trick the CF and force a new deployment to get back the CF stack to the previous stable state.

## Conclusion
In this tutorial, we covered all the needed steps to fix *ECS* services that are deployed through the *CloudFormation* template which is stuck in `UPDATE_ROLLBACK_FAILED` state. Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.