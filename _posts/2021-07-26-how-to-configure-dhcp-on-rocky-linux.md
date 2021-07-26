---
layout: post
title:  "How to configure DHCP to fix the network connectivity on Rocky Linux"
categories: [ rocky linux ]
image: assets/images/rocky-dhcp.jpg
tags: [ rocky linux, dhcp, networking ]
---
Some of the `RHEL` distros can cause common networking configuration issues on the first installation, so you will not be able to update the packages on a server or browse external Websites from your laptop. Centos was one of them, but now `Rocky Linux` is facing the same networking issue on his first boot. Once the `Rocky Linux` has been installed on your server or laptop the default network config is not configured to use `DHCP`. In this tutorial, I will show you the steps on how to configure `DHCP`, and get network connectivity.

## Prerequisites
* Rocky Linux installed
* sudo access

## Configure DHCP on Rocky Linux
**Step 1**. Once you have installed a new fresh instance of `Rocky Linux` make sure that the `NetworkManager service` is up and running:
```bash
systemctl status NetworkManager
```
`Output:`
```bash
● NetworkManager.service - Network Manager
   Loaded: loaded 
   Active: active (running) since Mon 2021-07-26 23:20:23 CEST; 50min ago
     Docs: man:NetworkManager(8)
 Main PID: 1225 (NetworkManager)
    Tasks: 4 (limit: 4915)
```

**Step 2**. NetworkManager applies a configuration read from the files found in `/etc/sysconfig/network-scripts/ifcfg-<IFACE_NAME>`. Each network interface has its configuration file. On your first boot the default config should look like this:
```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
NAME=eth0
UUID=95a13599-42cf-43d8-9fc1-d7c8b8d282dd
DEVICE=eth0
ONBOOT=yes
IPADDR=192.168.0.1
PREFIX=24
GATEWAY=192.168.0.xxx
DNS1=192.168.0.254
DNS2=1.1.1.1
IPV6_DISABLED=yes
```

**Step 3**. In the above `/etc/sysconfig/network-scripts/ifcfg-<IFACE_NAME>` listing, we can see that the value of the `BOOTPROTO` parameter is set to `none`. This means that the system being configured is set to a static IP address scheme. In order to establish network connectivity, you will have to change the value of the `BOOTPROTO` parameter from `none` to `DHCP` and also remove the `IPADDR`, `PREFIX`, and `GATEWAY` lines, because all of that information will be automaically obtained from any available DHCP server. The config should look like:
```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=eth0
UUID=95a13599-42cf-43d8-9fc1-d7c8b8d282dd
DEVICE=eth0
ONBOOT=yes
```
The `ONBOOT` parameter set to `yes` indicates that this connection will be activated during boot time. Once you are done, save the file, reboot the machine, and you should have network access.

## Conclusion
This tutorial shows you how to quickly resolve the network connectivity on your first installation of `Rocky Linux` using `dhcp`.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.