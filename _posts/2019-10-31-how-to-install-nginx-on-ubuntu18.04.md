---
layout: post
title:  "How to install Nginx on Ubuntu 18.04"
categories: [ nginx ]
image: assets/images/nginxfinal.jpg
tags: [nginx, cloud, ubuntu, proxy, linux]
---
Nginx was created in 2004 by a Russian developer Igor Sysoev as he was frustrated with Apache and wanted to build a replacement capable of handling 10000 concurrent connections with the focus on high performance, high concurrency and low memory usage. Today Nginx serves the majority of the world's top 1000 websites and whilst this growth is largely due to its performance it's also because Nginx is relatively easy to get started with. Of course it's by no means a simple piece of software but it's really good at making practical tasks such as caching or video streaming. Very easy to implement and there's a large number of first and third party modules which helps to extend its core functionality. However it's important to understand that Nginx is a reverse proxy server at its core.

## Prerequisites
* Make sure that you are logged into your Ubuntu 18.04 machine as a user with `sudo` privileges.

{% include in-article-ad.html %}

## About the C10k Problem 
The C10k problem is the problem of optimising network sockets to handle a large number of clients at the same time. The name C10k is a numeronym for concurrently handling ten thousand connections. Note that concurrent connections are not the same as requests per second, though they are similar: handling many requests per second requires high throughput (processing them quickly), while high number of concurrent connections requires efficient scheduling of connections.

## Comparation Nginx VS Apache 
* `Apache` is configured in what's called prefork mode. That means that he had spawned a set number of processors and each of which can serve a single request at a time regardless is that request for php or an image.

* `Nginx` on the other hand deals with the requests asynchronously meaning that a single nginx process can serve multiple request concurrently. Because Nginx has asynchronous design unlike Apache can't embed server site programming, meaning that all requests for Dynamic Content have to be dealt by a completely separate process like PHP-FPM(he is not build-in module in nginx, he is
a solved like reverse proxy).

## Installing Nginx with a package manager
The first installation method that I will cover will be via the operating system built-in package manager. Before we start please note that this is a very quick and easy solution and it's also very limited. It doesn't allow us to add any extra modules or funcionality to `Nginx`.
As we already know Ubuntu ships with the `APT` package manager, so first we will update our system by running:
```bash
sudo apt-get update
```
Now all the packages are up to date, so we can install `Nginx` by running
```bash
sudo apt-get install nginx
```
It will ask your for some additional dependency packages to be install. Type `yes`.
After the installation is done the Nginx process is also up and running. We can confirm this by running:
```bash
sudo systemctl status nginx
```

## Building Nginx from source
Before we start bulding Nginx from source, as a good practise is to update all the packages:
```bash
sudo apt-get update
```
The next step is downloading the latest Nginx sourcecode.
Now before we start navigating the Nginx websites note that we have both `nginx.org` and `nginx.com`. `Nginx.org` is where we'll look for the majority of our documentation, and `nginx.com` is the flashier product side of Nginx. Although we'll also have to visit it for some resources.

To download the latest source Nginx version you can visit [the following Nginx page](http://nginx.org/en/download.html). You can use `wget` or `curl` to download it on your Ubuntu machine. We will use `wget`:
```bash
sudo wget http://nginx.org/download/nginx-1.17.5.tar.gz
```
Next, extract the package with the following command:
```bash
sudo tar -zxvf nginx-1.17.5.tar.gz
```
Navigate to the nginx extracted directory:
```bash
cd nginx-1.17.5.tar.gz
```
**Step 1**. Configuring our source with the configure script in the sourcecode directory. First we need to install a compiler for Nginx, so the `./configure` script can work:
```bash
sudo apt-get install build-essential
```
Now we have all the necessary tools to compile Nginx. But if you run the script again you will see that other dependencies are missing (libpcre). You can install them by running:
```bash
sudo apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
```
If you run the script `./configure` again it will run without any errors.

**Step 2**. To find all the available configuration options run:
```bash
./configure --help
```
 To get a detail description of each of those configuration visit the [Nginx official documentation](http://nginx.org/en/docs/configure.html).
  Let's install some methods for `Nginx` and buid it from source. To install Nginx parametar methods run:
  ```bash
  ./configure 
        --sbin-path=/usr/bin/nginx 
        --conf-path=/etc/nginx/nginx.conf 
        --error-log-path=/var/log/ngix/error.log
        --pid-path=/usr/local/nginx/nginx.pid
        --with-http_ssl_module
        --with-pcre=../pcre-8.43
        --with-zlib=../zlib-1.2.1
```
Once the custom configuration is done, so we can compile this configuration source by running `make`. Please notice that this might take a quite a bit of time:
```bash
make
```
Once it is done install the compile source with `make install` :
```bash
make install 
```
To make sure that everything is Ok, check the configuration files that we installed at:
```bash
ls -l /etc/nginx
```
If they are all there, you have succesfully installed `Nginx` from source. You can also verify this by checking the Nginx version:
```bash
nginx -V
```
The output should be:
```bash
nginx version: nginx/1.17.5
```

## Conclusion
In this tutorial we learned about the Nginx. Why to use Nginx instead of Apache and how to install on Ubuntu 18.04 from a package manager and build it from source. For more details check the [Nginx documentation](http://nginx.org/).
