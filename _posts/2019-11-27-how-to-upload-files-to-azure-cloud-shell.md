---
layout: post
title:  "How to upload files to Azure Cloud Shell"
categories: [ azure ]
image: assets/images/files-azure-cloud-shell.jpg
tags: [ azure, azure-cli ]
---
As the name says, Azure Cloud Shell is a browser-accessible shell, which is used for managing Azure resources. It comes with two types of command shells **Bash** and **PowerShell**. It also gives you the flexibility to choose and switch between them. Although, it is primarily accessible from the browser, you could also [Add Azure Cloud Shell to Visual Studio Code]({% post_url 2019-09-25-add-azure-cloud-shell-to-vscode %}){:target="_blank"}.  
In order to use the Azure Cloud Shell, you must first create a storage account. Once you've created it, on the first run, Azure will prompt to create an Azure File Share, so the data will persist between sessions. This could be quite helpful sometimes, for example when you need to upload files to work with them later.  
There are currently two available methods to do this, using the Cloud Shell itself, or the Azure Portal.

## Prerequisites
* Azure account
* Azure storage account

{% include in-article-ad.html %}

## Upload using Azure Portal
**Step 1**. Login to the Azure Portal, and type *storage accounts* in the search textbox.  
![Search textbox](/assets/images/screenshots/screenshot49.png)

**Step 2**. Click on the Cloud Shell Storage account.  
![Cloud Shell storage account](/assets/images/screenshots/screenshot50.png)  
Note: If you have multiple storage accounts, the Cloud Shell storage account should be located under **cloud-shell-storage-region** *Resource group*.

**Step 3**. The Cloud Shell is using the Azure File Storage, so click on the **File shares**.  
![Azure File shares](/assets/images/screenshots/screenshot51.png)

**Step 4**. Open the File share.  
![Cloud Shell file share](/assets/images/screenshots/screenshot52.png)

**Step 5**. Open the **.cloudconsole** folder and click *Upload*.  
![Cloud Shell upload](/assets/images/screenshots/screenshot53.png)

**Step 6**. It'll display a side panel on the right side. Choose a file to upload, and click **Upload**.  
![Cloud Shell upload panel](/assets/images/screenshots/screenshot54.png)

## Upload using Cloud Shell
**Step 7**. Login to the Azure Portal, and click on the **Cloud Shell** icon.  
![Cloud Shell icon](/assets/images/screenshots/screenshot55.png)

**Step 8**. Click on the *Upload/Download files* button, and click Upload.  
![Cloud Shell upload/download](/assets/images/screenshots/screenshot56.png)

## Conclusion
Persisting files in Azure Cloud Shell is a good practice, especially if you are switching between hosts and don't want to copy or move local files everytime you need them.  
More about [Persist files in Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/persisting-shell-storage){:target="_blank"}.