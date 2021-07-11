---
layout: post
title:  "How to install Ansible on Rocky Linux 8"
categories: [ ansible ]
image: assets/images/ansible-rocky.jpg
tags: [ ansible, rockylinux, linux, cli ]
---
If there is a need to simplify the deployments of your applications, manage your infrastructure servers, etc.. you can achieve it with ansible which stands as an IT automation tool that provides a lot of features and integrations with all of the cloud providers and the CI/CD tools.

## Install Ansible on Rocky Linux
**Step 1**. Run system update command:
```bash
sudo dnf update
```

**Step 2**. Enable `EPEL` repository :
```bash
sudo dnf install epel-release
```

**Step 3**. Install Ansible:
```bash
sudo dnf install ansible
```

**Step 4**. Verify the installation: 
```bash
ansible --version
```
`Output`:
```bash
ansible 2.9.23
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.6/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.6.8 (default, May 19 2021, 03:00:47) [GCC 8.4.1 20200928 (Red Hat 8.4.1-1)]
```

## Uninstall Ansible on Rocky Linux
**Step 1**. If you decide to remove ansible on Rocky Linux execute:
```bash
dnf remove ansible
```

## Conclusion
This tutorial shows you how to install the latest ansible version on Rocky Linux 8, and if you decide to use other automation tool instead of ansible there is a section on how to remove it depending on your needs.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.