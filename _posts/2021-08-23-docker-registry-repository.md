---
layout: post
title:  "Docker Registry vs Docker Repository"
categories: [ docker ]
image: assets/images/docker-registry-repository.png
tags: [ docker, registry, repository ]
---
Docker is one of that technologies that will probably stay for a while. In a matter of years, the IT world went from monolith to deploying applications on the edge in a couple of seconds, using none other then containers. Docker being leader in the world of containers, is enough reason to start learning, and get your hands dirty. In today's blogpost, aimed for beginners, we are going to see what's the difference between Docker Registry and Docker repository.

## Prerequisites
* Docker

## Docker repository
You can think of Docker repository as a GitHub repository, but instead of storing code, it stores multiple versions of your Docker image, by having different tags. `So, it's basically a collection or logical grouping of Docker images with the same name and the same purpose`. And the question now is, where do we get or create this Docker repositories?! You guessed it right, the Docker registry. 

## Docker Registry
Docker registry is a Docker repository hosting platform that stores your Docker images, so you can push, pull, or remove images. These registries could be hosted by a third party vendors (usually cloud provider giants) as public or private registries. For example:

* Docker Hub
* AWS ECR
* Azure Container Registry
* Google Container Registry

Or, you can host it by yourself:

* Docker local registry
* Harbor 
* Quay

## Conclusion
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.