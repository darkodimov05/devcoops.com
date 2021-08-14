---
layout: post
title:  "How to set up Nginx reverse proxy for your application"
categories: [ nginx ]
image: assets/images/nginx-reverse-proxy.jpg
tags: [ nginx ]
---
Using Nginx reverse proxy could be very useful, especially if you are developing an application with a framework that can run its own server like nodejs, python, ruby on rails, etc... Running a node server on your local machine with a specific port it's a good practice for a testing purpose, but when it comes to deploying it on a production environment you might be looking for a reverse proxy and nginx is the right choice of doing that.

## Prerequisites
* Nginx

## Solution
**Step 1**. Open the Nginx config directory:
```bash
sudo cd /etc/nginx/
```

**Step 2**. You need to update the `vhost` file of your application. The vhost config file is usually stored under `sites-available/` or `conf.d/` subdirectory. If you want to proxy all the application requests update the root `location` block with the following config:
```bash
server {
    ...
    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         http://localhost:port;
    }
    ...
}
```

* `proxy_set_header :` Allows redefining or appending fields to the request header passed to the proxied server.
* `proxy_pass :` Sets the protocol and address of a proxied server and an optional URI to which a location should be mapped. The address can be specified as a domain name or IP address, and an optional port.

**Step 3**. Test the Nginx configuration and reload:
```bash
nginx -t
nginx -s reload
```

## Conclusion
As I mentioned previously, creating an Nginx proxy can cover complex scenarios depending on your requirements, but in this tutorial, you are going to learn how to set up nginx reverse proxy for your application which listens on a specific port.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.