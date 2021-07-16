---
layout: post
title:  "How to install Docker on Rocky Linux 8"
categories: [ docker ]
image: assets/images/docker-rockylinux8.jpg
tags: [ docker, rockylinux, linux ]
---
In the early December last year, RedHat took us all by suprise with the CentOS 8 End Of Life (EOL) [announcement](https://blog.centos.org/2020/12/future-is-centos-stream){:target="_blank"}, that was scheduled for the end of 2021. In response to this, the original CentOS founder has created a fresh project which is called Rocky Linux. The project quickly gained it's popularity among many clients of the cloud computing giants AWS, Azure and Google Cloud. This is a great opportunity for the distro to proof docker capabilities. In the following steps, we will see how we can quickly install and setup Docker CE.

## Prerequisites
* Rocky Linux 8

## Add Docker repository
**Step 1**. Update system OS packages
```bash
dnf update -y
```

**Step 2**. Add Docker repository
```bash
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
```  
`Output:`
```bash
Adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
```

**Step 3**. We can confirm it by listing current enabled repositories
```bash
dnf repolist
```  
`Output:`
```bash
repo id                 repo name
appstream               Rocky Linux 8 - AppStream
baseos                  Rocky Linux 8 - BaseOS
docker-ce-stable        Docker CE Stable - x86_64
extras                  Rocky Linux 8 - Extras
```

## Install Docker
**Step 4**. Install Docker
```bash
dnf install -y docker-ce docker-ce-cli containerd.io
```

**Step 5**. Enable Docker to start on boot
```bash
systemctl enable --now docker
```
`Output:`
```bash
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.
```

**Step 6**. Start Docker service
```bash
systemctl start docker
```

## Verify and test Docker installation
**Step 7**. To verify docker service running, type the following command
```bash
systemctl status docker
```

**Step 8**. As a best practice, we should always run docker as a non root user (user without sudo privileges)
```bash
usermod -aG docker $USER
```

**Step 9**. Confirm that you are part of the `docker` group
```bash
groups
```
`Output:`
```bash
devcoops docker
```

**Step 10**. Make sure you can run docker commands
```bash
docker --version
```
`Output:`
```bash
Docker version 20.10.7, build f0df350
```

**Step 11**. Let's test things out by spinning up a `hello-world` container
```bash
docker run --rm --name test-me hello-world
```

## Conclusion
Installing Docker on Rocky Linux 8 should be an easy procedure and it's kinda the same as installing on any other Linux distribution. In some of the future posts, we will cover on how you can install docker-compose as well.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.