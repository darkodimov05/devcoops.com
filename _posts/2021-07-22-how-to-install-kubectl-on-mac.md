---
layout: post
title:  "How to install kubectl on macOS"
categories: [ mac ]
image: assets/images/brew-mac.jpg
tags: [ kubernetes, macos, kubectl ]
---
Kubectl is a powerful command-line tool that allows you to control your `kubernetes` clusters. With `kubectl` you can deploy applications, manage your cluster recourses, and view logs. This tutorial will help you to install kubectl on your `macOS`.

## Prerequisites
* Brew

## Install Kubectl with the Brew package manager
**Step 1**. Open a terminal and update Brew:
```bash
brew update
```

**Step 2**. Upgrade new packages if any:
```bash
brew upgrade
```

**Step 3**. Install `kubectl`:
```bash
brew install kubectl 
```

**Step 4**. Verify the installation:
```bash
kubectl version --client
```

## Uninstall Kubectl
```bash
brew uninstall kubectl
```

## Conclusion
There are multiple ways to install `kubectl` on macOS, but the easiest and more effective way is to do it through `brew`.
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.