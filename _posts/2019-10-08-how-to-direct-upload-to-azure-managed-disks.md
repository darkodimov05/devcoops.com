---
layout: post
title:  "How to Direct-Upload to Azure Managed Disks"
categories: [ azure ]
image: assets/images/azure-direct-upload.jpg
tags: [ azure, azure-cli ]
---
A few days ago, Microsoft announced the preview of direct-upload to Azure Managed Disks. So, there are two ways you could move a VHD to the Azure cloud as a managed disk:
1. Upload the VHD in a storage account, and then convert it into a managed disk.
2. Create and attach empty managed disk to a VM, and then do the copy.

As stated by [Raman Kumar](https://azure.microsoft.com/en-us/blog/author/ramankum/){:target="_blank"}, a Principal Project Manager at Azure, these two ways have disadvantages. The first scenario requires extra storage account management, while the second scenario has extra cost because of a running virtual machine. Direct-upload allows copying an on-premise VHD directly to the Azure cloud as a managed disk, which reduces the cost and the management issues mentioned earlier.  
Currently, you can direct-upload to standard HDD, standard SSD, and premium SSD managed disks of all supported sizes.

Direct-Upload can be approached using [**Azure Storage Explorer**](https://azure.microsoft.com/en-us/features/storage-explorer/){:target="_blank"} via GUI, which is basically a standalone app, or if you are more of a developer, or DevOps guy, you could use a Rest API or Azure CLI with AzCopy command-line utility. I have already covered AWS to Azure migration topic on this [post]({% post_url 2019-10-03-how-to-copy-from-aws-s3-bucket-to-azure-blob-storage %}){:target="_blank"}. Today, we are going to use Azure CLI in combination with AzCopy.

Check the official blog [Introducing the preview of direct-upload to Azure managed disks](https://azure.microsoft.com/en-us/blog/introducing-the-preview-of-direct-upload-to-azure-managed-disks){:target="_blank"}.

## Prerequisites
* Azure account
* Azure Storage account

{% include in-article-ad.html %}

## Create an Azure Managed Disk
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "managed-disks-rg" --location westeurope
```

**Step 3**. Create a empty managed disk:  
```bash
az disk create --name "myfirstmanageddisk" --resource-group "managed-disks-rg" --location westeurope --for-upload --upload-size-bytes 34359738880 --sku "standard_lrs"
```
Parameters:
* `--name`: name of the managed disk.
* `--resource-group`: name of the managed disk's resource group.
* `--location`: location of the managed disk.
* `--for-upload`: creates a disk for uploading purposes.
* `--upload-size-bytes`: the file size (in bytes) of the VHD you want to upload.
* `--sku`: storage SKU.

**Step 4**. Next, create a writable shared access signature (SAS) of your empty managed disk:  
```bash
az disk grant-access --name "myfirstmanageddisk" --resource-group "managed-disks-rg" --access-level Write --duration-in-seconds 86400
```
The parameters are self-explanatory. Output will be in json format. For example:
```json
{
"accessSas": "https://md-impexp-wjjjpktbq2st.blob.core.windows.net/vh3sfg1tc3tj/abcd?sv=2017-04-17&sr=b&si=c71a65c4-1a3a-49c0-9f5b-d884b8c6c384&sig=cvswkCDM5tW4SNJe7g9O0CNaVYaIgr4IHoKzi7NP3dA%3D"
}
```

**Step 5**. Now that you have writable SAS, you can upload a VHD using AzCopy tool:  
```bash
azcopy copy "VHDs/managedDisk1.vhd" "<insert_the_sas_token_output_here>" --blob-type PageBlob
```

**Step 6**. Once the upload completed, you can revoke the SAS, so it allows you to attach a managed disk to a VM:     
```bash
az disk revoke-access --name "myfirstmanageddisk" --resource-group "managed-disks-rg"
```

Now, you can attach the managed disk to a virtual machine.

## Cleaning up

**Step 7**. Delete the managed disk:   
```bash
az disk delete --name "myfirstmanageddisk" --resource-group "managed-disks-rg"
```

**Step 8**. Delete the resource group:  
```bash
az group delete --name "managed-disks-rg"
```

## Conclusion
If you are working with IaaS Virtual Machines, using Direct-Upload is a recommended way to do it, because it will save you a lot of time in a backup and restore, disaster and recovery scenarios.