---
layout: post
title:  "How to secure Nginx with Let's Encrypt on Rocky Linux 8"
categories: [ certbot, nginx ]
image: assets/images/nginx-certbot.jpg
tags: [ nginx, security, certbot, rockylinux ]
---
Previously we saw [how to install Certbot on Rocky Linux 8](https://devcoops.com/how-to-install-certbot-on-rocky-linux/) depending on your web server, whether it's Nginx or Apache. In this tutorial, I'm going to show you how to secure your Nginx web server with a free Let's Encrypt SSL certificate. Let's start.

## Prerequisites
* sudo access
* Rocky Linux 8
* [Nginx installed](https://devcoops.com/how-to-install-nginx-on-rocky-linux/)
* [Certbot installed](https://devcoops.com/how-to-install-certbot-on-rocky-linux/)

## Generate Let's Encrypt SSL Certificate
**Step 1**. Before obtaining a Let's Encrypt SSL certificate, make sure that your domain is correctly pointed to your server IP address and propagated. There is an online [dns tool](https://www.whatsmydns.net/) that you can use to check it.

**Step 2**. Run the following command to obtain Let's Encrypt certificate trough certbot:
```bash
sudo certbot --nginx -d domain.com -d www.domain.com
```
* `certbot` - this will run certbot
* `--nginx` - it's a certbot plugin that we want to use it.
* `-d`      - specify the names that you'd like the certificate to be valid for.

**Step 3**. If the command is successful, certbot will ask how you’d like to configure your HTTPS settings:
`Output:`
```bash
Please choose whether HTTPS access is required or optional.
-------------------------------------------------------------------------------
1: Easy - Allow both HTTP and HTTPS access to these sites
2: Secure - Make all requests redirect to secure HTTPS access
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

Select your choice and hit enter. Once the certificate is generated you should get the following `Output:`
```bash
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/domain.com/fullchain.pem. Your cert will
   expire on xxxx-xx-xx. To obtain a new or tweaked version of this
   certificate in the future, simply run certbot again with the
   "certonly" option. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work
 ```
 That means that you have successfully generated SSL for your domain.  

 **Step 4**. You need to reload Nginx so the changes can take effect:
 ```bash
 sudo systemctl reload nginx
```
Now try to reload your website and notice your browser’s security indicator. It should represent that the site is properly secured, usually with a green lock icon.

## Conclusion
This tutorial shows you how to secure Nginx with Let's Encrypt free SSL certificate.  Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.