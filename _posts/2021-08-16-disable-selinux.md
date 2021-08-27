---
layout: post
title:  "How to disable SELinux"
categories: [ linux ]
image: assets/images/selinux-disable.jpg
tags: [ linux, selinux  ]
---
SELinux is a security architecture for Linux systems that allows administrators to have more control over who can access the system. The SELinux Policy is the set of rules that guide the SELinux security engine. It defines types for file objects and domains for processes. Sometimes you may get permission issues due to SELinux, so in this tutorial I will cover the steps on how to disable selinux temporarily and permanently.

## Prerequisites
* Linux Distro

## Disable SELinux Temporarily
**Step 1**. Check the selinux status:
```bash
sestatus
```

**Step 2**. If it's enabled execute the following command to disable it temporarily:
```bash
setenforce 0
```

## Disable SELinux Permanently
**Step 1**. Open the selinux configuration file:
```bash
nano /etc/sysconfig/selinux
```

**Step 2**. Find the `SELINUX` configuration line and change the directive `SELINUX=enforcing` to `SELINUX=disabled`
```bash
SELINUX=disabled
```

**Step 3**. Save the file and exit. You need to `reboot` the system if you want the changes to take effect. Check the new status:
```bash
sestatus
```
`Output:`
```bash
SELinux status: disabled
```

## Conclusion
This is a quick and easy tutorial where you can learn how to disable selinux temporarily or permanently depending on your requirements.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.