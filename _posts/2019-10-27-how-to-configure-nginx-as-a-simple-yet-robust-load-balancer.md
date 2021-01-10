---
layout: post
title:  "How to configure Nginx as a simple and robust load balancer"
categories: [ nginx ]
image: assets/images/nginxend.jpg
tags: [nginx, cloud, reverse proxy, proxy, linux]
---
Nowadays load balancers are mostly used for improving the distribution of workloads across multiple computing resources, improving the scalability, redundancy and the deliverability of the resources. In short, a load balancer should achieve two main objectives:

* The ability to distribute requests to multiple servers thus reducing the load on those individual servers
* Provide redundancy

Providing redundancy is when one or more of our load balanced servers fail for whichever reason, the load balancer needs to recognize that and redirect the requests to any of the remaining available servers. When we say distribute or redirect of course we mean on `proxy`.

## Prerequisites
* Already installed [nginx web server](https://devcoops.com/how-to-install-nginx-on-ubuntu18.04/)
* Installed php on your machine
* Make sure that you are logged into your Linux machine as a user with `sudo` privileges

{% include in-article-ad.html %}

## Creating multiple PHP servers
To demonstrate the usage of the `Nginx` load balancer I will create a multiple PHP servers. I will simply have them each return and identifying plain message.

**Step 1**. First let's create a directory where we will store the PHP servers and create them separately:
```bash
mkdir -p /var/www/php_servers
cd /var/www/php_servers
```

**Step 2**. Now we will create the first server using the command line interface. We will store the information in the `s1` file, and there is no need for an extension here because the PHP server doesn't care about the input file type.
```bash
echo 'PHP Server 1' > s1
```
Using the above `echo` command we will create the others two servers:
```bash
echo 'PHP Server 2' > s2
echo 'PHP Server 3' > s3
```
Now to make sure that are created you can list them using:
```bash
ls -l 
```
The `output` should be similar to this:
```php
-rw-r--r-- 1 root root   13  27 15:00 s1
-rw-r--r-- 1 root root   13  27 15:02 s2
-rw-r--r-- 1 root root   13  27 15:02 s3
```
**Step 3**. Next we will start the servers separately with the following commands in a separete console window: 
```bash
php -S localhost:10001 s1
php -S localhost:10002 s2
php -S localhost:10003 s3 
```
To make sure that the servers are up and running, we will test the first one with the following command: 
```bash
curl http://localhost:10001
```
If everything is Ok you should see :
```bash
PHP Server 1
```
Of course in a real world example these servers would much more likely be serving the exact same data given the same requests, but we will have to distinguish them in order to test our load balancer.

## Setting up Nginx as a load balancer
First open a new console window and navigate to the `Nginx` default configuration : 
```bash 
cd /etc/nginx
nano nginx.conf
```
Remove the whole content in the `nginx.conf` configuration file and paste the following:
```nginx 
user www-data;
worker_processes auto;

events {
}
http {

  upstream php_servers {

    server localhost:10001;
    server localhost:10002;
    server localhost:10003;
  }
  server {

        listen 8888;
        server_name localhost;
        root /var/www/php_servers;

        location / {

          proxy_pass http://php_servers;
        }

   }

}
```
Let's explain the configuration:

* `upstream` This is a contex or block in Nginx server that group several servers with the ability to add some options.
* `listen` we are telling the Nginx server to listen to the `8888` specific port, default is `80`.
* `location /` This means all requests for / go to the any of the servers listed under `upstream php_servers`, with a preference for port.

**Step 1**. To monitor the load balancing we are going to issue these `curl` commands to the `nginx` server using a simple `while` loop on the command line : 
```bash
while sleep 0.5; do curl http://localhost:8888; done
```
So basically we are creating a loop which sleeps for half a second on every iteration and then performs the `curl` command on our Nginx load balancer.

If you executed the `while` loop you should see the output like below:
```bash
PHP Server 1
PHP Server 2
PHP Server 3
PHP Server 1
PHP Server 2
PHP Server 3
PHP Server 1
PHP Server 2
PHP Server 3
```
So Nginx is balancing our requests by sending them to each page piece server in turn. This default behavior is reffered to as `Round Robin`, meaning to simply send the next request to the next server in the `upstream`.

**Step 1**. As we mentioned before the second purpose of a load balancer is to provide us with some `redundancy`.
We can test this by killing one or two of our PHP servers. We will kill the server1 with the followinng command:

```bash 
kill -9 [process_id]
```
You can find the `process_id` by using the `ps` command.

After that you will get `output` like this:
```bash
PHP Server 2
PHP Server 3
PHP Server 2
PHP Server 3
PHP Server 2
PHP Server 3
PHP Server 2
PHP Server 3
```
## Conclusion
In this tutorial we shown you how to use and configure Nginx as a simple and robust load balancer. Nginx can be configured with some different variants of load balancing. For more details you can visit the [Nginx load balancing documentation](http://nginx.org/en/docs/http/load_balancing.html).