---
layout: post
title:  "How to create a Wordpress Stack using Docker Compose"
categories: [ docker ]
image: assets/images/wpdocker.jpg
tags: [docker, docker compose, wordpress, lamp]
---
Wordpress is an open source content management system (CMS) written in PHP and it's the easiest and most powerful blogging and website CMS in the world. Dockerizing the Wordpress stack will help you to set up a Wordpress stack in just a few minutes instead of installing it from scratch. In this tutorial, we will cover the steps needed for setting up a Wordpress stack using docker-compose.

## Prerequisites
Before continuing with this tutorial you need to met the following prerequisites:

* Make sure that you are logged into your Ubuntu machine as a user with `sudo` privileges 
* Docker installed. You can use the instructions from [How to install docker on Ubuntu 19.04](https://devcoops.com/install-docker-on-ubuntu-19.04/) 
* Docker Compose already installed. Follow the instructions from [How to install Docker Compose on Ubuntu 19.04](https://devcoops.com/install-docker-compose-on-ubuntu-19.04/)

{% include in-article-ad.html %}

## Create the environment 
Before creating the Docker Compose file we will create a directory which will be used to deploy the application. 
```bash
mkdir ~/wordpress
```
Now we will switch to the `wordpress` directory and start creating the the stack.
```bash
cd ~/wordpress
```
## Configure the Stack
Once you are into the `wordpress` directory, we are ready to create a docker compose file and configure the stack. First we will create the docker compose file:
```bash
touch docker-compose.yml
```
Now open the file and paste the following configuration:
```yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    container_name: db
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    env_file: 
      - .env
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: wordpress
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}

  wordpress:
    image: wordpress
    container_name: wordpress
    restart: always
    volumes:
      - ./wp_data:/var/www/html
    ports:
      - "52:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: ${MYSQL_USER}
      WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
    depends_on:
       - db
  
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: always
    ports:
      - '80:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    depends_on:
      - db
volumes:
    db_data:
```
Now lets explain the `docker compose` file step by step:

* The first line is showing the Compose file version.
* Next, we are defining three services, `db`, `wordpress` and `phpmyadmin`. We are using phpmyadmin, so you can simply manage your databases.

The db service:
* We are using `mysql:5.7 image`. If the image is not present on the local system it will be pulled from the Docker Hub repo.
* Uses the restart always policy. Whenever the Docker service is restarted, containers using the always policy will be restarted regardless of whether they were running or now.
* Creates persistent volume `dbdata`.
* Defines the environment variables for the mysql:5.7 image which will be stored in the `.env` file separately.

The wordpress service:

* Uses the wordpress image. 
* Uses the restart always policy. Whenever the Docker service is restarted, containers using the always policy will be restarted regardless of whether they were running or now.
* Mounts the wp_data directory on the host to `/var/www/html` inside the container.
* Expose port `80` on the container to port `52` on the host machine.
* Defines the environment variables for the wordpress image which will be stored in the `.env` file separately.
* `depends_on` instruction is defining the dependency between the two services. So db will be started before wordpress.

The phpmyadmin service:
* Uses the phpmyadmin image. 
* Uses the restart always policy. Whenever the Docker service is restarted, containers using the always policy will be restarted regardless of whether they were running or now.
* Expose port `80` on the container to port `80` on the host machine.
* Defines the environment variables for the wordpress image which will be stored in the `.env` file separately.
* `depends_on` instruction is defining the dependency between the two services. So db will be started before phpmyadmin.

Next, lets create the `env` file and store the variables:
```bash
touch .env
```
Open the file and put the following variables:
```bash
MYSQL_ROOT_PASSWORD=changethepass
MYSQL_USER=changeuserpass
MYSQL_PASSWORD=passshouldbechanged
```
We successfully configured the stack, next is to deploy and build the images.

## Deploy the stack

To build the docker images execute  the following command:
```bash
docker-compose up
```
Please note that this command should be executed in the same directory where the `docker compose` file is.

Check if the containers are up and running with the following command:
```bash
docker ps
``` 
To access the wordpress instance, type in browser:
```bash
http://localhost:52 
``` 
To access the phpmyadmin instance, type in browser:
```bash
http://localhost
```
To stop the containers, type the following command:
```bash
docker-compose stop
```

To remove the containers, type the following command:
```bash
docker-compose down  
```

## Conclusion
In this tutorial we shown you `How to create a Wordpress Stack using Docker Compose` and if you have some further questions or information please put a comment in the box below.  