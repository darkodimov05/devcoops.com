---
layout: post
title:  "How to create and configure Azure Bastion"
categories: [ azure ]
image: assets/images/azure-bastion.jpg
tags: [ azure, azure-vm ]
---
A bastion host, or also known as a Jumpbox, is a server which is exposed on the public (demilitarized zone) network for a special purpose. The bastion host is designed and configured to withstand attacks, and to be accessible from the outside world, usually by using SSH or RPD, so you can safely connect to the private network, or the backend services. The server has a small attack surface by hosting a single application, like a proxy server, and running only the necessary services. Although, when we think of a bastion host, we mean a server that is accessible using SSH or RDP, a bastion host could also be:  
* DNS server
* Email server
* FTP server
* VPN server
* Proxy server
* Web server

Azure Cloud came up with a Platform-as-a-service (PaaS) this year, known as `Azure Bastion`. **Azure Bastion** is a fully managed service that allows you to securely access yours Azure Virtual Machines over SSH/RDP directly from the Azure Portal. It brings advantages:
* No need for RDP or SSH client on your local machine, connect directly from the Azure Portal
* No need for local RDP or SSH ports
* No public IP required on the Azure VM
* Works with modern browsers on any device
* Can replace an existing Bastion Host

![Source: Microsoft Docs](/assets/images/screenshots/screenshot39.png)
*Source: Microsoft Docs*

## Prerequisites
* Azure account
* Azure VM

{% include in-article-ad.html %}

## Create an Azure Bastion host
**Step 1**. Open the Azure Portal and search the Marketplace for *Bastion*.
![Azure Bastion marketplace](/assets/images/screenshots/screenshot34.png)

**Step 2**. Click **Create**.

**Step 3**. On the **Project details**, create new *Resource group*. For example *azure-bastion-rg*.
![Azure Bastion project-details](/assets/images/screenshots/screenshot35.png)

**Step 4**. On the **Instance Details**, enter the *Name* of the instance. For example: *azure-bastion*.
![Azure Bastion instance-details](/assets/images/screenshots/screenshot36.png)

**Step 5**. On the **Configure virtual networks**, add an existing virtual network, or create a new one. Also, make sure the `Azure Bastion` subnet name is **AzureBastionSubnet** with a subnet mask at least /27. Create new public IP address and enter a *Public IP address name*.
![Azure Bastion virtual-networks](/assets/images/screenshots/screenshot37.png)

**Step 6**. Click **Review + create**.
![Azure Bastion review](/assets/images/screenshots/screenshot38.png)

**Step 7**. Click **Create**.

## Connect with Azure Bastion

**Step 8**. Under *Virtual Machines* from the main Azure Portal window, click on the VM name that you want to connect to.

**Step 9**. Click **Connect**. It will open up a new panel on the right side. Click **BASTION**.

**Step 10**. Enter the *Username* and under *Authentication Type*, choose *SSH Private Key* or *SSH Private Key from Local file. Click **Connect**.
![Azure Bastion connect](/assets/images/screenshots/screenshot40.png)

**Step 11**. A new console will be open in another browser tab.
![Azure Bastion connect](/assets/images/screenshots/screenshot41.png)

## Cleanup
**Step 12**. Delete the resource group using the Azure CLI:  
```bash
az group delete --name "azure-bastion-rg"
```

More about [Create an Azure Bastion host](https://docs.microsoft.com/en-us/azure/bastion/bastion-create-host-portal){:target="_blank"}.

## Conclusion
From a security aspect `Azure Bastion` could be quite useful, if you don't want to expose ports to the public network. But, it's not so practical and productive copying and pasting keys from the clipboard to the Azure Portal, especially if you are fan of the Terminal, or even working as DevOps.