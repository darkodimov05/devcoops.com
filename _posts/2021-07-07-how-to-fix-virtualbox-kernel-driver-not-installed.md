---
layout: post
title:  "How to fix kernel driver not installed (rc=-1908) on Mac"
categories: [ mac ]
image: assets/images/mac-vbox-error.jpg
tags: [ mac, virtualbox ]
---
The first time you try to spin up a VM on Mac, you will probably go with VirtualBox as a Hypervisor, and honestly VBox is pretty good for beginners who just want to get the things done. It's simple, easy to use, open-source, and the most popular one. But, the first time you try to start a VM, you will most probably get the following error:  
```bash
Kernel driver not installed (rc=-1908)

Make sure the kernel module has been loaded successfully.

where: suplibOsInit what: 3
VERR_VM_DRIVER_NOT_INSTALLED (-1908)
- The support driver is not installed. On
linux, open returned ENOENT.
```  
Let's see how can we fix it in two steps.

## Prerequisites
* VirtualBox

## Resolve VBox error steps
**Step 1**. First, go to `System Preferences / Security`

**Step 2**. Unlock the settings and under `General / Allow apps downloaded from:` click the button `Allow`. Lock the settings again.  
![Mac Allow apps downloaded from](/assets/images/screenshots/screenshot57.png)

## Conclusion
At first, you might not see the `Allow` button, wait for 30 minutes and then try again. If the `Allow` button is still not shown, try to uninstall VirtualBox either by running the `VirtualBox_Uninstall.tool` which can be found when you mount the VBox disk image, or manually remove any leftover files under `/Library/` path. Then, try to run a fresh installation again.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.