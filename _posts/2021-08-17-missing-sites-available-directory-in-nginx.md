---
layout: post
title:  "Missing sites-available directory in Nginx"
categories: [ nginx ]
image: assets/images/nginx-sites-available-missing.jpg
tags: [ nginx ]
---
As part of the [Nginx](https://devcoops.com/categories/#nginx){:target="_blank"} series, in today's tutorial, we are going to see on how to add that missing `sites-available` subdirectory. The reason behind this, is because most of us have been comfortable using Debian or Ubuntu, and the `sites-available`/`sites-enabled` logic is not used by the upstream packaging of nginx by default. So, let's see how can we implement this logic in the next few steps.

## Prerequisites
* Nginx

## Solution
**Step 1**. Create the missing subdirectories:
```bash
sudo mkdir -p /etc/nginx/{sites-available,sites-enabled}
```

**Step 2**. Open `/etc/nginx/nginx.conf` and add the following line in the `http` block:
```bash
http {
    . . .
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

**Step 3**. Create a vhost config file in `sites-available` subdirectory:
```bash
touch /etc/nginx/sites-available/default
```

**Step 4**. Once you add the configuration, create a soft link:
```bash
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
```

**Step 5**. Test the nginx configuration:
```bash
sudo nginx -t
```

**Step 6**. Restart the nginx service:
```bash
sudo systemctl restart nginx
```

**Step 7**. Get yourself a beer. You deserve it.

## Conclusion
Although, `sites-available`/`sites-enabled` logic seems more convenient, as a best practice, use default `conf.d`, just because is a standard convention and can work anywhere. And, if you need to disable a site, you could just rename the config by adding a `.disabled` suffix. For example:  
```bash
sudo mv -i /etc/nginx/conf.d/default.conf{,.disabled}
```
If you need to re-enable it, just rename the file by removing the `.disabled` suffix:
```bash
sudo mv -i /etc/nginx/conf.d/default.conf{.disabled,}
```
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.