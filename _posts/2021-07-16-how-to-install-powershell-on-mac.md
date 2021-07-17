---
layout: post
title:  "How to Install PowerShell on Mac"
categories: [ mac ]
image: assets/images/mac-powershell.jpg
tags: [ powershell, macos ]
---
As a devops engineer, we most likely would prefer to work with our cool and comfy MacBook Pros. And if you are working with Azure, you will most probably need to install PowerShell at some point. There are few ways to do it, so let's see how we can install PowerShell the easiest way possible.

## Prerequisites
* Brew

## Install PowerShell via the famous package manager Brew
**Step 1**. Open a terminal and update Brew:
```bash
brew update
```

**Step 2**. Upgrade new packages if any:
```bash
brew upgrade
```

**Step 3**. Install PowerShell:
```bash
brew install --cask powershell
```

**Step 4**. Verify the installation:
```bash
pwsh
```
Output:
```bash
➜  ~ pwsh
PowerShell 7.1.3
Copyright (c) Microsoft Corporation.

https://aka.ms/powershell
Type 'help' to get help.

PS /Users/devcoops> 
```

## Uninstall PowerShell
```bash
brew uninstall --cask powershell
```

## Conclusion
I would always consider installing any kind of software on MacOS via Brew. It's more effective to handle things via package manager instead of bunch of .pkg files scattered around the file system.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.