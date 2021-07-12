---
layout: post
title:  "How to install Docker Compose on ARM64"
categories: [ docker ]
image: assets/images/dockercompose-arm64.jpg
tags: [ docker, linux, arm64 ]
---
In some of the previous posts I've covered how you can install docker-compose on Linux distributions like [Rocky Linux]({% post_url 2021-07-06-how-to-install-docker-compose-on-rocky-linux %}){:target="_blank"} for example. It's pretty easy to set it up, right?! Usually when we spin up instances in the cloud we are working with x86_64 processors instruction set under the hood. But, if we try to install docker compose on instance running on arm64 following the default installation instructions, we'll get an error.   
This is because there is no docker-compose binary available for arm64 instruction set, atleast not in the latest release at the time of writing, which is version [v.1.0.17](https://github.com/docker/compose-cli/releases/tag/v1.0.17){:target="_blank"}. There are a few ways we can get docker-compose installed and running on arm64. Let's see it in the following steps below.  
**Note**: Compose v2 (pre-release) has arm7 and arm64 binaries [available](https://github.com/docker/compose-cli/releases/tag/v2.0.0-beta.3){:target="_blank"}.

## Prerequisites
* Arm64 Linux distribution

## Install Compose using pip
You need to have `pip` or `pip3` installed. Then, just run the following code:
```bash
pip install docker-compose
or
pip3 install docker-compose
```

## Install Compose as a container
Install Compose as a container using a bash script wrapper:
```bash
sudo curl -L --fail https://github.com/docker/compose/releases/download/1.29.2/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## Uninstall Compose
Uninstall Compose installed via `pip` or `pip3`:
```bash
pip uninstall docker-compose
or
pip3 uninstall docker-compose
```

Uninstall Compose installed via `curl`:
```bash
sudo rm /usr/local/bin/docker-compose
```

## Conclusion
Installing Docker Compose on arm64 is just as easy as the x86_64 one. At the end of the day, I think it might be more convinient to install and run docker-compose as a container instead as a binary just because we almost treat everything as a microservice in the cloud, or we can simple wait Compose v2 to be stable and ready as a non-beta release.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.