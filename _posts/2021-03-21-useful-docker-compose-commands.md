---
layout: post
title:  "Useful docker and docker-compose commands"
categories: [ docker ]
image: assets/images/docker-compose.jpeg
tags: [ docker, docker-compose ]
---
Working with docker-compose in a development environment can be challenging sometimes for the newcomers, unless you got the time on your hands to google everything from stack overflow and save it somewhere locally. In this post, I'll share with you some commands that might help you from the start. The words like `services` and `containers` will be used interchangeably.


## Prerequisites
* Docker  
* Docker Compose  

{% include autoads.html %}

## Docker Compose
**Build and start docker-compose stack in the background**
```bash
docker-compose up --build -d
```  
This command will build or rebuild services (if you have changes in a service Dockerfile or in it's build directory)in the docker-compose stack and run them in the background. This is usually useful when trying to deploy the stack for the first time. `-d` stands for detached mode.

**Stop and remove all docker containers in the docker-compose stack**
```bash
docker-compose down
```
This command will stop and remove all services (containers) specified in the docker-compose stack. If you want to remove the volumes and images used by any service add `-v` and `--rmi all` parameters as well.

**Restart a single docker-compose service**
```bash
docker-compose restart <service_name>
```
If you ommit a service name, it will restart all the services.

**Start docker-compose stack with a scaled up service**
```bash
docker-compose up -d --scale <service_name>=5
```

**Update containers that have newer image than currently running**
```bash
docker-compose up -d --no-deps <service_name>
```

**Get docker-compose to pull the latest images from repositories**
```
1. docker-compose down
2. docker-compose pull (for remote repositories) and/or docker-compose build --pull (for local build images)
3. docker-compose up -d
```

Official docker-compose [commandline documentation](https://docs.docker.com/engine/reference/commandline/compose/).

## Docker
**Run alpine docker container in a detached mode**
```bash
docker run -dit --name test-container alpine
```

**Get into the docker container shell**
```bash
docker exec -it <container_name_or_id> /bin/bash
```
**Note**: if `/bin/bash` doesn't work, try with `/bin/sh`.

Parameters:
* `-it or -i -t`: allocating a tty for the container process, or
 * `-i`: keeps stdin open even if it's not attached
 * `-t`: allocate a pseudo-tty

**Get info about the docker image or container**
```bash
docker inspect <image_name_or_container_name>
```

**Show docker container logs**
```bash
docker logs <container_name_or_id>
```

If you want to stream the logs:
```bash
docker logs --follow <container_name_or_id>
```

If you want to display only the latest lines:
```bash
docker logs --tail 100 <container_name_or_id>
```

**Get docker container IP address**
```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name_or_id>
```

**Clean out docker images, builds, volumes, containers, ...**
```bash
docker system prune --volumes
```
This command will remove all stopped containers, build cache, dangling images, networks that are not used by at least one container, and not used volumes.

Remove all custom networks only not used by at least one container:
```bash
docker network prune
```

Remove all volumes only not used by at least one container:
```bash
docker volume prune
```

Remove all stopped containers:
```bash
docker container prune
```

Remove all containers including its volumes use (YOLO):
```bash
docker rm -vf $(docker ps -aq)
```

Remove all images (YOLO):
```
docker rmi -f $(docker images -aq)
```

Official docker [commandline documentation](https://docs.docker.com/engine/reference/commandline/docker/).

## Conclusion
I'm sure there's a lot more commands to be added, so feel free to leave a comment below.
