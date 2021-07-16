---
layout: post
title:  "Clean up Docker space for Jenkins jobs"
categories: [ docker, jenkins ]
image: assets/images/dockerjenkins.jpg
tags: [ docker, jenkins, docker space ]
---
Managing and deploying complex infrastructure with Jenkins sometimes can cause job failure issues. For example, if you are working with microservices and all the deployments are going through the Jenkins server that definitely will cause space issues especially if the Jenkins is inside a docker container. The docker images that you are building for your microservices will fill up the data blocks within containers, so you will run into a docker space issue.

## Prerequisites
* CLI access of your dockerized Jenkins instance

## Check Docker Data Space
**Step 1**. To check the docker data space on your dockerized Jenkins instance execute:
```bash
sudo docker info | grep "Data Space"
```
If you don't have enough `available` space or you just have 1 or 2 GB left you can proceed with the steps below.

## Cleaning up Docker space
**Step 1**. Check if you have a non-running containers on the server. If so then execute:
```bash
sudo docker rm $(sudo docker ps -aq)
```
**Step 2**. Now check if you have some unused images and remove them with the following command:
```bash
sudo docker rmi $(sudo docker images -q)
```
**Step 3**. If removing the unused images and the non-running containers didn't free up the docker space memory then you will have to remove the unused data blocks within containers. You can use the following command to run `fstrim` on any running container and discard any data blocks that are unused by the container file system:
```bash
sudo sh -c "docker ps -q | xargs docker inspect --format='{{ .State.Pid }}' | xargs -IZ fstrim /proc/Z/root/"
```

## Conclusion
I wouldn't recommend using Jenkins in a dockerized environment, but if you already have it for your infra and it is causing space issues, you can set up a `cronjob` in Jenkins with the commands above.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.