---
layout: post
title:  "How to install Terraform on Rocky Linux"
categories: [ terraform ]
image: assets/images/rocky.jpg
tags: [ terraform, cloud, rocky, linux, rockylinux ]
---
Rocky Linux was kicked off by CentOS co-founder Gregory Kurtzer in December after CentOS's Linux parent company and the goal is to replace CentOS cause RedHat announced it would shift focus from CentOS Linux to CentOS Stream. So it turns out that the decision becomes popular cause the distro was downloaded at least 10,000 times within half a day of its release. In this tutorial I'll cover the steps on how to install terrafrom on Rocky Linux 8.

## Prerequisites
* Rocky Linux 8
* sudo access

## Install Terraform on Rocky Linux 8
**Step 1**. Install `yum-config-manager` so you can manage your repos:
```bash
sudo yum install yum-utils
```
**Step 2**. Now we will use the `yum-config-manager` to add the official HashiCorp Linux repository:
```bash
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
```
**Step 3**. Install it: 
```bash
sudo yum install terraform
```
**Step 4**. Verify the installation and the latest version with the following command:
```bash
terraform  version
```
`Output:`
```bash
Terraform v1.0.1
```

## Conclusion
This tutorial shows you how to install Terraform CLI on Rocky Linux 8. For more info please visit the official documentation at [ terraform documentation ](https://www.terraform.io/docs/index.html).  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.