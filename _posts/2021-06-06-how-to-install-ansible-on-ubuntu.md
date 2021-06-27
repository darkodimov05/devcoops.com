---
layout: post
title:  "How to install Ansible on Ubuntu"
categories: [ ansible, ubuntu ]
image: assets/images/ansible-ubuntu-install.jpg
tags: [ ansible, ubuntu ]
---
Ansible stands as an IT automation tool that can simplify the deployments of your applications, managing your infrastructure servers, and provides a lot of features and integrations with all the cloud providers and the CI/CD tools.

## Install Ansible on Ubuntu
**Step 1**. Update the Ubuntu packages
```bash
sudo apt update
```
**Step 2**. Install `software-properties-common` package to easily manage your distribution and independent software vendor software sources like PPAs:
```bash
sudo apt install software-properties-common
```
**Step 3**. Now we need to add `ppa:ansible/ansible` to our systems software source list, so we can install the latest version of ansible:
```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
```
**Step 4**. To install the latest version of Ansible run:
```bash
sudo apt install ansible
```
**Step 5**. Make sure that you have the latest Ansible version installed:
```bash
ansible --version
```

---
**NOTE:**
If you are installing Ansible on older Ubuntu distros, `software-properties-common` is called `python-software-properties` and you may need to user `apt-get` instead of `apt`

---

## Remove Ansible from Ubuntu
**Step 1**. If you want to delete all the configuration files and data use:
```bash
sudo apt-get purge ansible
```
**Step 2**. If you need to remove all the configuration files and data associated with the ansible package use this command, but keep in mind that you can can't recover the data, so, use this command with care: 
```bash
sudo apt-get purge --auto-remove ansible
```

## Conclusion
This tutorial shows you how to install the latest ansible version on Ubuntu, and if you decide to use other automation tool instead of ansible there is a section on how to remove it depending on your needs.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.