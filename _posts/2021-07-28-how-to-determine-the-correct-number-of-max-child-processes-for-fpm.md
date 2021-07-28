---
layout: post
title:  "How to determine the correct number of max children processes for PHP-FPM"
categories: [ php-fpm ]
image: assets/images/php-fpm.jpg
tags: [ php, php-fpm, nginx ]
---
Most of the PHP developers are using Nginx with php-fpm as the most efficient approach. Most likely nginx will throw php errors, that the server reached the `pm.max_children` number. It means that probably you haven't set up the number correctly or you are using the default value. In this tutorial, I will explain how to determine the correct number of max children processes.

## Prerequisites
* php and php-fpm installed
* nginx

## Determine the correct number of child processes for php-fpm
**Step 1**. Firstly you will have to find the correct path of the php-fpm configuration file depending on your OS. On Debian-based distro you can find it at `/etc/php/fpm/pool.d/www.conf`, open the file and find the `pm.max_children` line.

**Step 2**. To get the correct value for the number of pm.max_children you will have to check how much memory your server can use for the php-fpm process and divide that total by the average size in Mb of your fpm processes. The following command will show you the average memory usage in kilobytes per process in the RSS column:
```bash
ps -ylC php-fpm --sort:rss
```
`Output:`
```bash
S   UID   PID  PPID  C PRI  NI   RSS    SZ WCHAN  TTY          TIME CMD
S     0  8550  8452  0  80   0 31800 142192 SyS_ep ?       00:00:02 php-fpm
S     0 14648 14548  0  80   0 31868 142196 SyS_ep ?       00:00:29 php-fpm
S     0 32356 32258  0  80   0 32040 142196 SyS_ep ?       00:00:25 php-fpm
S  1000 28358  8550  0  80   0 36400 143952 inet_c ?       00:00:00 php-fpm
S  1000 28359  8550  0  80   0 36456 143935 inet_c ?       00:00:00 php-fpm
S  1000 28153  8550  0  80   0 36652 144003 inet_c ?       00:00:00 php-fpm
S  1000 27850  8550  0  80   0 36940 144071 inet_c ?       00:00:00 php-fpm
S  1000 27948 14648  0  80   0 43356 144993 inet_c ?       00:00:03 php-fpm
S  1000 27933 32356  0  80   0 47588 145732 inet_c ?       00:00:03 php-fpm
S     0 30312 30212  0  80   0 48228 193672 SyS_ep ?       00:01:11 php-fpm
S  1000 27932 32356  0  80   0 50800 146100 inet_c ?       00:00:03 php-fpm
S  1000 27930 32356  0  80   0 53124 145818 inet_c ?       00:00:03 php-fpm
```

**Step 3**. The next command will calculate the average memory usage for all above fpm processes in MB:
```bash
ps --no-headers -o "rss,cmd" -C php-fpm | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'
```
`Output:`
```bash
37Mb
```

**Step 4**. So the approach is `max_children = System_RAM / Average FPM Pool size`. Let's say if your server has 4GB RAM and you decide to give 2GB memory for the FPM processes the result will be:
```bash
max_children = 2048 MB / 37 MB = 55.351 ~ (50)
```
Approximately you should set the `pm.max_children` number to 50.

## Conclusion
This tutorial shows you how to determine the correct number of max children processes for php-fpm.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.