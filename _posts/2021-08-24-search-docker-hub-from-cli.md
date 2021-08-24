---
layout: post
title:  "How to search Docker Hub repositories from CLI"
categories: [ docker ]
image: assets/images/dockerhub-cli.jpg
tags: [ docker, docker hub, cli, linux ]
---
The `docker search` command lets you search Docker Hub from the CLI. This has limited value as you can only pattern-match against strings in the `NAME` field. However, you can filter output based on any of the returned columns. In its simplest form, it searches for all repos containing a certain string in the `NAME` field.

## Prerequisites
* Docker installed

## Searching Docker Hub repos from CLI
The following command will list all repositories that include the string `alpine` in the name with all their information:
```bash
sudo docker search alpine
```
`Output:`
```bash
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
alpine                                 A minimal Docker image based on Alpine Linux…   7782      [OK]       
mhart/alpine-node                      Minimal Node.js built on Alpine Linux           483                  
anapsix/alpine-java                    Oracle Java 8 (and 7) with GLIBC 2.28 over A…   472                  [OK]
frolvlad/alpine-glibc                  Alpine Docker image with glibc (~12MB)          267                  [OK]
alpine/git                             A  simple git container running in alpine li…   189                  [OK]
yobasystems/alpine-mariadb             MariaDB running on Alpine Linux [docker] [am…   95                   [OK]
alpine/socat                           Run socat command in alpine container           75                   [OK]
kiasaki/alpine-postgres                PostgreSQL docker image based on Alpine Linux   44                   [OK]
jfloff/alpine-python                   A small, more complete, Python Docker image …   41                   [OK]
zenika/alpine-chrome                   Chrome running in headless mode in a tiny Al…   34                   [OK]
byrnedo/alpine-curl                    Alpine linux with curl installed and set as …   34                   [OK]
hermsi/alpine-sshd                     Dockerize your OpenSSH-server with rsync and…   34                   [OK]
...
```
* `NAME:` The NAME field is the repository name

## Searching only official Docker Hub repos from CLI
From the previous command, you can notice how some of the repositories returned are official and some are unofficial. You can use --filter
`is-official=true` so that only official repos are displayed:
```bash
sudo docker search alpine --filter "is-official=true"
```
`Output:`
```bash
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
alpine    A minimal Docker image based on Alpine Linux…   7782      [OK]
```

## Searching only automated Docker Hub repos from CLI
If you need to filter only repos with `automated builds`, run:
```bash
sudo docker search alpine --filter "is-automated=true"
```
`Output:`
```bash
NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
anapsix/alpine-java                    Oracle Java 8 (and 7) with GLIBC 2.28 over A…   472                  [OK]
frolvlad/alpine-glibc                  Alpine Docker image with glibc (~12MB)          267                  [OK]
alpine/git                             A  simple git container running in alpine li…   189                  [OK]
yobasystems/alpine-mariadb             MariaDB running on Alpine Linux [docker] [am…   95                   [OK]
alpine/socat                           Run socat command in alpine container           75                   [OK]
kiasaki/alpine-postgres                PostgreSQL docker image based on Alpine Linux   44                   [OK]
jfloff/alpine-python                   A small, more complete, Python Docker image …   41                   [OK]
hermsi/alpine-sshd                     Dockerize your OpenSSH-server with rsync and…   34                   [OK]
byrnedo/alpine-curl                    Alpine linux with curl installed and set as …   34                   [OK]
zenika/alpine-chrome                   Chrome running in headless mode in a tiny Al…   34                   [OK]
hermsi/alpine-fpm-php                  FPM-PHP 7.0 to 8.0, shipped along with tons …   25                   [OK]
etopian/alpine-php-wordpress           Alpine WordPress Nginx PHP-FPM WP-CLI           25                   [OK]
bashell/alpine-bash                    Alpine Linux with /bin/bash as a default she…   18                   [OK]
roribio16/alpine-sqs                   Dockerized ElasticMQ server + web UI over Al…   15                   [OK]
davidcaste/alpine-java-unlimited-jce   Oracle Java 8 (and 7) with GLIBC 2.21 over A…   13                   [OK]
spotify/alpine                         Alpine image with `bash` and `curl`.            11                   [OK]
cfmanteiga/alpine-bash-curl-jq         Docker Alpine image with Bash, curl and jq p…   6                    [OK]
hermsi/alpine-varnish                  Dockerize Varnish upon a lightweight alpine-…   3                    [OK]
goodguykoi/alpine-curl-internal        simple alpine image with curl installed no C…   1                    [OK]
dwdraju/alpine-curl-jq                 Alpine Docker Image with curl, jq, bash         1                    [OK]
bushrangers/alpine-caddy               Alpine Linux Docker Container running Caddys…   1                    [OK]
smartentry/alpine                      alpine with smartentry                          0                    [OK]
```

## Conclusion
Using `docker search` command sometimes can help you to get a brief list of the Docker image repos instead of visiting Docker Hub web page. However one last thing about `docker search` . By default, Docker will only display 25 lines of results. Essentially you can use the `--limit` flag to increase that to a maximum of 100. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.