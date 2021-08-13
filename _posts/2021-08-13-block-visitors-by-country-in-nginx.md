---
layout: post
title:  "How to block visitors by country in Nginx"
categories: [ nginx ]
image: assets/images/nginx-block-countries.jpg
tags: [ nginx ]
---
In some of mine previous posts, I wrote about how you can [whitelist IPs in Nginx]({% post_url  2021-08-09-whitelist-ips-in-nginx %}){:target="_blank"}. In today's tutorial, we are going to see how can we block specific countries in a few steps.

## Prerequisites
* Nginx

## Solution
**Step 1**. First, check if your Nginx version supports the `HttpGeoipModule`:
```bash
nginx -V
```

In the output, you should look for `--with-http_geoip_module`.

**Step 2**. Install the GeoIP database:
```bash
sudo apt-get install geoip-database libgeoip1 -y
```

**Step 3**. Configure Nginx by updating the `nginx.conf` file. Let's say we want to allow the traffic  that comes from US only. Place the following code in the `http` block:
```bash
http {
    ...
    geoip_country /usr/share/GeoIP/GeoIP.dat;
    map $geoip_country_code $allowed_country {
        default no;
        US yes;
   }
}
```

You can find the list of country codes [here](https://dev.maxmind.com/geoip/legacy/codes#iso-3166-country-codes){:target="_blank"}.

**Step 4**. The second part of the Nginx configuration comes from updating the vhost config file that's usually stored under `sites-available/` or `conf.d/` subdirectory. Add the following block in either `server` block, or `location` block (if you want to restrict just a certain path of the site). For example:
```bash
server {
    ...
    if $(allowed_country = no) {
        return 403;
    } 
}
```

**Step 5**. Test the Nginx configuration and reload:
```bash
nginx -t
nginx -s reload
```

## Conclusion
As always, consider using the `HttpGeoipModule` only if you can't deal with firewalls for some reason.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.