---
layout: post
title:  "How to install Podman on Ubuntu 20.04"
categories: [ ubuntu, podman ]
image: assets/images/podman-ubuntu.png
tags: [ podman, ubuntu, linux ]
---
If you try to install Podman on Ubuntu 20.04 with the default Ubuntu packages, the installation will fail with the following error: `E: Unable to locate package podman`. The podman package is on a PPA repository which needs to be added prior to installation. In this tutorial, I'm going to show you how to install podman on Ubuntu 20.04

## Prerequisites
* Ubuntu 20.04
* sudo access

## Install Podman on Ubuntu 20.04
**Step 1**. Update the packages:
```bash
sudo apt update
```

**Step 2**. Run the following command to source release version
```bash
source /etc/os-release
```

**Step 3**. Execute the following commands to add the PPA repository as we mentioned previously:
```bash
echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
curl -L "https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/Release.key" | sudo apt-key add -
```

**Step 4**. Once the repo is added update the packages:
```bash
sudo apt update
```

**Step 5**. Install podman:
```bash
sudo apt -y install podman
```

**Step 6**. Verify the installation:
```bash
podman info
```

## Conclusion
This tutorial will guide you on how to install podman on Ubuntu 20.04. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.
