---
layout: post
title:  "How to install Docker Compose on Ubuntu 19.04"
categories: [ docker ]
image: assets/images/dockercom.jpg
tags: [ docker, docker compose, ubuntu, linux ]
---
Docker Compose is a tool for defining and running multiple Docker applications. With Docker Compose you use a compose file to configure your application's services then using a single command to create and start all the services from your configuration. 
We can use Compose as a single host application deployment, for local development, automated testing, etc... 

So in this tutorial, we will see how to install the latest version of Docker Compose on Ubuntu 19.04 and also the same instructions apply for any other Debian based ditribution.

## Prerequisites
Before continuing with this tutorial you need to met the following prerequisites:

* Make sure that you are logged into your Ubuntu 19.04 machine as a user with `sudo` privileges
* Docker installed. You can use the instructions from [How to install docker on Ubuntu 19.04](https://devcoops.com/install-docker-on-ubuntu-19.04/) 

## Install Docker Compose 
Docker compose can be installed From the official Ubuntu 19.04 repositories, but to make sure that you are using the latest version of Docker Compose we will install it from the Docker's GitHub repository.
The latest verison of Docker Compose is `1.25.0` at the time of writing this article.

First we will download the Docker Compose binary into the `/usr/local/bin` directory:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
When the download process is finished we will apply the the executable permissions to the Composer binary:
```bash 
sudo chmod +x /usr/local/bin/docker-compose
```
To make sure that the Docker Compose is installed run the Compose vesion command:
```bash
docker-compose --version
```
You should get output like this:
```bash
docker-compose version 1.25.0, build 0a186604
```
We have sucessfuly installed Docker Compose on Ubuntu 19.04 using the Docker's GitHub repository. Now you can use it and build multiple Docker applications.

## Uninstalling Docker Compose

If you want to remove the Docker Compose for any reason you can simply execute the following command:
```bash
sudo rm /usr/local/bin/docker-compose
```
## Conclusion
In this tutorial we shown you `how to install Docker Compose on Ubuntu 19.04` and if you have some further questions please put a comment in the box below. For more information about using docker compose you can find in the [docker official documentation](https://docs.docker.com/compose/) 