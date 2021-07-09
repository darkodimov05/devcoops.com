---
layout: post
title:  "How to install Docker Compose on Rocky Linux 8"
categories: [ docker ]
image: assets/images/compose-rocky-8.jpg
tags: [docker, rockylinux, linux]
---
Installing Docker Compose on Rocky Linux follows the same procedure as the other Linux distros. Docker Compose relies on Docker Engine so first, you need to make sure that you have installed [Docker](https://devcoops.com/how-to-install-docker-on-rocky-linux/) on your Rocky Linux machine. So once the Docker is set up on your machine you can proceed with the instructions about the Docker Compose.

## Prerequisites
* Rocky Linux 8
* [Docker](https://devcoops.com/how-to-install-docker-on-rocky-linux/)

## Install Compose on Rocky Linux 8
**Step 1**. Run the `curl` command to download the current stable release of Docker Compose:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
If you want to install different version change the number `1.29.2` from the link above. You can find the versions at [Compose repository release page](https://github.com/docker/compose/releases)

**Step 2**. Once the download is finished apply executable permissions to the binary:
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

**Step 3**. Create a symbolic link to `/usr/bin` directory in your path:
```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

**Step 4**. Verify the installation:
```bash
docker-compose --version
```
`Output:`
```bash
docker-compose version 1.29.2, build 5becea4c
```

## Uninstall Compose on Rocky Linux 8
**Step 1**. If you decide to uninstall Docker Compose execute:
```bash
sudo rm /usr/local/bin/docker-compose
```

## Conclusion
Installing Docker Compose on Rocky Linux 8 is an easy procedure and if you followed the steps above you should have successfully installed Compose on `Rocky Linux 8`.
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.
