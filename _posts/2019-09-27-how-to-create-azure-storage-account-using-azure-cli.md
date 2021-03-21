---
layout: post
title:  "Create Azure storage account using Azure CLI"
categories: [ azure ]
image: assets/images/azure-cloud-shell-storage-account.png
tags: [azure, azure-cli]
---
An Azure storage account is used to store all of your Azure data objects. For example: blobs, files, disks, tables, and queues. Even though the storage account is publicly accessible over HTTP or HTTPS using endpoints, it is securable, highly available, durable and scalable.  

## Types of storage accounts
There are different types of storage accounts with different features, pricing models which surves different purposes. It always depends on what kind of scenario we are building when choosing the storage account types. There are currently 5 types of storage accounts:
* *General-purpose v2 accounts*: This is the default storage account and it's recommended for most scenarios.  
* *General-purpose v1 accounts*: The legacy version of the general-purpose v2 accounts.  
* *Block blob storage accounts*: As the name says, it's used only for blobs with premium performance, where low storage latency and high transactions rates are required.  
* *FileStorage storage accounts*: It's the same as the **block blob storage accounts** regarding performance, except it's for files-only instead of blobs.  
* *Blob storage accounts*: The non-premium standard version of the **block blob storage accounts**. It's preferred to use the general-purpose v2 accounts over this one.  

More about [Azure storage accounts](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview).  

## Prerequisites
* Azure account  
* Azure CLI  

## Configuration
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.  

**Step 2**. Before we create a storage account we need to create a resource group.   
```bash
az group create --name storage-rg --location westeurope
```
The json output should look like the following:  
![Create resource groups using Azure CLI](/assets/images/screenshots/screenshot8.png)  
We could also confirm the resource group by using the following command:  
```bash
az group list --query "[?location=='westeurope']"
```

**Note**: Before we create the storage account, we could also replace location for the resource group by choosing another location available for the azure account. To list all available locations run the command:  
```bash
az account list-locations --output table
```

**Step 3**. Create the storage account:  
```bash
az storage account create --name devcoopsstorage1 --resource-group storage-rg --location westeurope --sku Standard_RAGRS --kind StorageV2
```
The parameters are pretty much self-explanatory, except for the *sku* parameter which can have one of the following values:  
Standard_LRS, Standard_ZRS, Standard_GRS, Standard_RAGRS, Standard_GZRS, Standard_RAGZRS.  
These values represent the different replication options. See more about [Storage replication options](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy).  

Once the command runs successfully, the terminal will generate a lot of json output. We could also confirm the storage account by using the following command:  
```bash
az storage account list --resource-group storage-rg
```

## Cleanup

If we aren't using the storage account anymore, we can delete the resource group using the following command:
```bash
az group delete --name storage-rg
```

Official documentation: [Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account).