---
layout: post
title:  "Install Azure CLI 2.0 on macOS with brew"
categories: [ azure ]
image: assets/images/azure-cli-macos-brew.png
tags: [ azure, azure-cli, macos ]
---
The `Azure CLI` is a command-line tool that let you manage the Azure resources from your local terminal. Using the terminal can bring a lot of advantages, like being more efficient and automating tasks with scripts. We will easily install this tool using the [homebrew package manager](https://brew.sh).  

The current version of the `Azure CLI` is **2.0.74**.  

## Prerequisites
* Brew  
* Python3 package  
* Azure account  

{% include in-article-ad.html %}

## Installation
**Step 1**. Open terminal and update brew:
{% highlight ruby %}
brew update
{% endhighlight %}

**Step 2**. Install Azure CLI with brew:
{% highlight ruby %}
brew install azure-cli
{% endhighlight %}

**Step 3**. Run the az command from terminal to make sure the installation was successful:
{% highlight ruby %}
az
{% endhighlight %}
![Azure CLI on macOS](/assets/images/screenshots/screenshot1.png)

**Step 4**. Login to the console with Azure credentials:
{% highlight ruby %}
az login
{% endhighlight %}

It should open the Azure login page from the default browser.  

For more information, check out the official documentation by Microsoft:  
[Install Azure CLI on macOS](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-macos?view=azure-cli-latest).