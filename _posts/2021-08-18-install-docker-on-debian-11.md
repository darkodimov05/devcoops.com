---
layout: post
title:  "How to install Docker on Debian 11"
categories: [ docker ]
image: assets/images/debian11-docker.jpg
tags: [ docker, debian, debian 11 ]
---
Debian 11.0 was released on August 14th, 2021, and marked as a stable version. The release included many major changes, particularly it comes with a new fresh `Homeworld` theme. This tutorial will show you the steps on how to install Docker on a new fresh Debian 11 instance.

## Prerequisites
* Debian 11

## Install latest Docker version on Debian 11
**Step 1**. Update the `apt` package index:
```bash
sudo apt-get update
```

**Step 2**. Allow `apt` to use a repository over HTTPS:
```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

**Step 3**. Add Docker’s official GPG key:
```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

**Step 4**. Execute the following command to set up the Docker stable repository:
```bash
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

**Step 5**. To install the latest Docker version, we should update the `apt` package index, and install the latest version of Docker Engine and containerd:
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## Install a specific Docker version on Debian 11
To install a specific Docker version you need to complete the above 4 steps (without executing `Step 5`) and run:
```bash
sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

## Uninstall Docker Engine on Debian 11
If you decide to uninstall docker for some reason run:
```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

## Conclusion
This tutorial shows you the installation Docker steps on a fresh released Debian 11 version, which is quite simple and easy. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.