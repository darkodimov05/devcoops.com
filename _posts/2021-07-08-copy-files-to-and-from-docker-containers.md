---
layout: post
title:  "How to copy files/folders to and from Docker Containers"
categories: [ docker ]
image: assets/images/docker-cp.jpg
tags: [docker, docker cp, terminal, cli]
---
Docker copy command allows you to copy files/folders from the container’s file system to the local machine or the reverse, from the local filesystem to the container. In this tutorial, I'm gonna cover these two scenarios.

## Prerequisites
* Docker 

## Copying files/folders from Local Machine to Docker Container
**Step 1**. To copy the file from your host machine to the docker container, you need to list the running containers and get the `id` of the desired container:
```bash
sudo docker ps
```
Once you have the `id` of the container, copy the file into the container. I will copy the file into the `/opt/` directory inside the container, but you can specify a different path depending on your needs: 
```bash
sudo docker cp /home/devcoops/devcoops.txt 7d3055dd224c:/opt/
```
---
**NOTE**

If you do not specify the `full path` of your host file/folder, you will get an error:
```bash
no such file or directory
```

---

**Step 2**. Copy a folder with all the files from your host machine to the docker container:
```bash
sudo docker cp /home/devcoops/devcoopsdocker/ 7d3055dd224c:/opt/
```

## Copying files/folders from Docker Container to Local Machine
**Step 1**. Copy a file from your docker container to your host machine. I will use the same docker container `id` from the previous step:
```bash
sudo docker cp 7d3055dd224c:/opt/devcoops.txt /home/devcoops/
```

**Step 2**. Copy a folder with all the files from your docker container to the host machine:
```bash
sudo docker cp 7d3055dd224c:/opt/devcoopsdocker/ /home/devcoops/
```

---
**NOTE**

If you try to copy `files/folders` from Docker Container to Docker Container something like:
```bash
sudo docker cp 7d3055dd224c:/opt/devcoops.txt b9733ffea0ad:/opt/
```
It will throw an error that there is no possibility to copy files/folders between docker containers

`Error:`
```bash
copying between containers is not supported
```
---

## Conclusion
Docker `cp` command behaves like the Unix `cp` command which is an easy operation. If you have any questions regarding the `docker cp` command feel free to leave a comment below and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.