---
layout: post
title:  "Build a Docker Image and push it to Docker Hub"
categories: [ docker ]
image: assets/images/dockerhub.jpg
tags: [ docker, dockerhub, docker image ]
---
Docker Hub is a remote repository which allows you to share docker container images with your team, customers, and the Docker community.
Within a Dockerfile you can have all the commands that are needed to assemble an image and push that image to a Docker Hub repo.  A single Docker Hub repository can hold many Docker images stored as tags. In this tutorial, I'm going to show you how to build and push an image to Docker Hub repo.

{% include autoads.html %}

## Prerequisites
* Docker installed
* Docker Hub account

## Build and push the image
**Step 1**. Firstly, you need to login to your Docker Hub account executing the `docker login` command in your terminal:
```bash
sudo docker login --username=dockerhubusername --password=dockerhubpassword
```
**Step 2**. Build the image by executing the `docker build` command:
```bash
sudo docker build -t $DOCKER_ACCOUNT/$DOCKER_REPO:$IMAGE_TAG .
```
* $DOCKER_ACCOUNT - Name of your account
* $DOCKER_REPO - Repo name
* $IMAGE_TAG - Your tag

**Step 3**. After building the image the next thing is to push the image to your hub repo by executing the `docker push` command: 
```bash
sudo docker push $DOCKER_ACCOUNT/$DOCKER_REPO:$IMAGE_TAG
```

## Conclusion
Knowing how to build and push an image to Docker Hub is a good practice, especially when you are doing an automatic deployment of your app using some of the CI/CD tools.