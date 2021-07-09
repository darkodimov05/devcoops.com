---
layout: post
title:  "How to install docker on Ubuntu 19.04"
categories: [ docker ]
image: assets/images/docker.jpg
tags: [ docker, cloud, ubuntu, linux ]
---
Docker is mainly a software development platform and a kind of virtualization technology that makes it easy for us to develop and deploy applications inside of neatly packaged virtual containerized environments. This means that apps run the same, no matter where they are of what machine they are running on, so your software stays system agnostic, making software simpler to use, less work to develop, and easy to maintain and deploy. These containers running on your computer or server act like little micro computers each with very specific jobs, with their own operating system and their own isolated CPU and network resources.
A developer will usually start by accessing the Docker Hub, and online cloud repository of docker containers and pull one containing a pre-configured environment for their specific programming language.

Docker is a form of virtualization but, unlike Virtual Machines the recourses are shared directly with the host. This will allow you to run many Docker containers where you may only be able to run a few virtual machines.

## Prerequisites
* Make sure that you are logged into your Ubuntu 19.04 machine as a user with `sudo` privileges

## Install Docker 
First we will update the local database of software and make sure that we have access to the latest revisions. To do this open the terinal and execute:
```bash
sudo apt-get update
```
If you have previously installed some old versions of docker, remove it and its dependent packages:
```bash
sudo apt remove docker docker-engine docker.io containerd runc
```
You need to import Docker repository GPG key:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Now we will add the Docker CE repository to Ubuntu:
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
To install Docker CE on Ubuntu 19.04 run:
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```
We succesfully installed the docker on Ubuntu 19.04. To verify the installation we can check the docker version:
```bash 
sudo docker version
```
You should get similar output like this:
```bash
Client: Docker Engine - Community
 Version:           19.03.5
 API version:       1.40
 Go version:        go1.12.12
 Git commit:        633a0ea838
 Built:             Wed Nov 13 07:29:52 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.5
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.12
  Git commit:       633a0ea838
  Built:            Wed Nov 13 07:28:22 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```
We can also check if the docker daemon is started with the following command:
```bash
sudo systemctl status docker
```
You should get similar output like this:
```bash
 docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2019-12-13 23:27:50 CET; 9min ago
     Docs: https://docs.docker.com
 Main PID: 9941 (dockerd)
    Tasks: 13
   CGroup: /system.slice/docker.service
           └─9941 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```           

## Docker command
The syntax of docker command is easy and simple to use followed by arguments.
The form of the docker command syntax is:
```bash
docker [option] [command] [arguments]
```
If you want to check all available subcommands, run:
```bash
sudo docker
```
The output will be similar like the following:
```bash
Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```
Also, if you want to check the system wide information about Docker, run:
```bash
sudo docker info
```
To check only the space available for the docker containers on your machine execute:
```bash
sudo docker info | grep "Data Space"
```

## Conclusion
In this tutorial we shown you `how to install docker on Ubuntu 19.04` and if you have some further questions please put a comment in the box below. For more information about using docker you can find in the [docker official documentation](https://docs.docker.com/)