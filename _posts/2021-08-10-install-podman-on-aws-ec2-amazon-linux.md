---
layout: post
title:  "How to install Podman on AWS EC2 Amazon Linux 2 instance"
categories: [ aws, podman ]
image: assets/images/podman-aws.png
tags: [ podman, aws, ec2, amazon linux ]
---
If you are looking for a Docker replacement then `Podman` is the right choice for you. The installation process is quick and easy. 
Podman is a daemonless and open-source Linux native tool, which means that you don't need to start or manage a daemon process like the Docker daemon. The commands that you use with Docker are similar for Podman. Images of Docker are compatible with Podman as well, which makes it very flexible. In this tutorial, I'm gonna show you how to install podman on AWS EC2 Amazon Linux 2 instance. 

## Prerequisites
* AWS account 
* EC2 instance with Amazon Linux 2 template

## Install Podman on AWS EC2 Amazon Linux 2 instance
**Step 1**. Login to the EC2 instance using ssh and update the packages:
```bash
sudo yum update
```

**Step 2**. Add the Kubic project repo which provides updated packages:
```bash
sudo curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/CentOS_7/devel:kubic:libcontainers:stable.repo
```

**Step 3**. Install the yum corp plugin to add or remove Copr repositories to the local system:
```bash
sudo yum install yum-plugin-copr
```

**Step 4**. Enable the SELinux policy for container runtimes:
```bash
sudo yum copr enable lsm5/container-selinux
```

**Step 5**. Install podman:
```bash
sudo yum install podman
```

**Step 6**. To verify the installation run:
```bash
podman version
```

## Conclusion
This tutorial will quickly guide you on how to install Podman on AWS EC2 Amazon Linux 2 instance. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.