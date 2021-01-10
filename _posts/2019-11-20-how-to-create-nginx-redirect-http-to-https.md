---
layout: post
title:  "How to create http to https Nginx redirect"
categories: [ nginx ]
image: assets/images/nginxredirect.jpg
tags: [nginx, cloud, ubuntu, proxy, linux]
---
As we know Nginx has a lot of great features, one of them is to redirect your website from `http` to `https`. Using https is more secure which means that the communication between the client and the server is encrypted in both directions. Redirecting the traffic from `http` to `https` is the most common task if you are working as a DevOps or System Administrator. Nginx as a reverse proxy web server offers a lot of redirect rules but in this tutorial, we will take a look at the few of them.

## Prerequisites
* Already installed [nginx web server](https://devcoops.com/how-to-install-nginx-on-ubuntu18.04/)
* Make sure that you are logged into your Linux server as a user with `sudo` privileges

{% include in-article-ad.html %}

## Redirect all to HTTPS
The easiest and the most common method is to redirect all the 80(HTTP) requests to 443(HTTPS).
To enable this you can copy and paste the following configuration:
```nginx
server {
    listen 80 default_server;
    server_name your-domain.com;
    return 301 https://$host$request_uri;
}
```
This server block listens and accepting the requests on the port 80 default server. The `server_name` should match your domain name and finally the last line is returning a 301 redirect to the `https` block of whatever URI is requested.

## Redirecting specific sites
If you have multiple sites and not all of them have SSL certificate, Nginx allows you to redirect only specific sites. To do that you should copy and paste the following configuration:
```nginx
server {
    listen 80 default_server;
    server_name your-domain.com;
    return 301 https://your-domain.com$request_uri;
}
```
This server block listens and accepting the requests only on the port 80. Also it is only listening for the requests made to the your-domain.com. And the last line returns 301 redirect to the `https` only for the hostname your-domain.com

## Https redirect www to non-www
First to set up this redirect on your domain please make sure that you have a valid `www` DNS record in your nameservers. If you don't have you can create one, copy and paste the following lines into your nginx configuration file:
```nginx
server {
    server_name www.your-domain.com;
    return 301 $scheme://your-domain.com$request_uri;
}
```
This tells nginx to redirect requests from `www.your-domain.com` to `your-domain.com`. So don't forget to execute:
```bash
sudo systemctl reload nginx
```
to put the changes into effect for all the above redirects.

## Redirect an existing URI to a new one
To redirect a specific URI of your domain the best practise is to create a `location` block. To make this work put the following configuration into your nginx conf file:
```nginx 
location ~* "^/some-specific-uri.html" {
    return 301 https://your-domain.com/the-desired-redirect-uri.html;
}
```
The above configuration will match all the requests that conatains `some-specific-uri.html` in the URL and redirect them to the `https://your-domain.com/the-desired-redirect-uri.html`. 

## Conclusion
In this tutorial we shown you a few most common redirects that will help you to manage your domain. If you like this tutorial please comment bellow or share it.