---
layout: post
title:  "How to whitelist IPs in Nginx"
categories: [ nginx ]
image: assets/images/nginx-whitelist-ips.jpg
tags: [ nginx ]
---
We all know what firewalls are for, but sometimes they can become a pain in the arse, and we all end up opening HTTP and HTTPS port by default. Let's say we host multiple domains via Nginx, and we want to test a website, which is not production-ready, so we need to restrict the domain to be available only from a single IP, or an IP subnet.

## Prerequisites
* Nginx

## Whitelist IPs for a domain
Open the nginx config file for that particular domain, and add the following block of lines:
```bash
server {
    ...
    allow 192.168.10.2;
    deny all;
    ...
}
```

## Whitelist an IP subnet
Pretty simple, right?! But, what if we want to whitelist an IP subnet? Add the following block of lines:
```bash
server {
    ...
    allow 192.168.10.1/24;
    deny all;
    ...
}
```

## Whitelist multiple IPs and/or subnets
Let's say we got a list of IPs that needs to be whitelisted. The configuration should look something like this:
```bash
server {
    ...
    allow 192.168.10.2;
    allow 192.168.10.3;
    allow 10.100.100.2/24;
    ...
    deny all;
    ...
}
```

This solution is inneficient, because what if we have 100 IPs from different geolocations and need to whitelist just a subset of the whitelisted IPs on specific domain subdirectory?! It will be a mess.

## Whitelist multiple IPs and/or subnets DRY-er version
```bash
geo $whitelisted_ip {
    default           0;

    192.168.10.11/32  1;
    192.168.15.15/24  2;
    ...
}

server
{
    if ( $whitelisted_ip = 0 ) {
        return 403;
    }
}
```

## Whitelist multiple IPs and/or subnet for a domain location (subdirectory)
What if we only want to restrict IPs to a certain domain subdirectory, for example `www.domain.com/secretlocation`. Basically, it's the same as restricting domain access on a root level.
```bash
geo $whitelisted_ip {
    default           0;

    192.168.10.11/32  1;
    192.168.15.15/24  2;
    ...
}

server
{
    server www.domain.com
    ...

    location /secretlocation {
        if ( $whitelisted_ip = 0 ) {
            return 403;
        }
    }
    ...
}
```

## Conclusion
Although Nginx has a cool feature to whitelist IPs, it should be mostly used in certain situations. There is nothing better then a firewall, and it'll probably take 5 minutes to set it up.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.