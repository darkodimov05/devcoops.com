---
layout: post
title:  "How to restore deleted Azure Storage Blobs"
categories: [ azure ]
image: assets/images/recover-azure-blob-storage.jpg
tags: [ azure, azure blob ]
---
Accidental deletion of files can happen to anyone working either on-premise or in the cloud. As a DevOps engineer who is responsible for managing access permissions to resources, the principle of least privilege is a must, which means limiting people and services access to bare minimum so they can perform their work. But, sometimes even if we want to protect the files as much as possible, a human error could happen. That's where **Soft delete** can be useful.

## What is **Soft delete**
**Soft delete** is an Azure Storage blob feature which can help you recover your accidental deleted blob objects by an application or storage account users. Currently, it's available to blob storage only including hot, cool and archive storage tiers.

## How it works
When data is deleted, the blob object transition to a soft deleted state instead of being permanently removed. And everytime when object is overwritten, a soft delete snapshot is generated in order to save the state of the overwritten object.  
![Blob Service diagram](/assets/images/screenshots/screenshot19.png)  
The recoverable time is configurable with maximum of one year. It is backward compatible which means you don't have to make any changes in the application code in order yo user it. It is billed at the same rate as active data.

## Prerequisites
* Azure account
* Azure storage account

{% include in-article-ad.html %}

## Enable Soft delete feature
**Step 1**. Login to the Azure Portal and go to the Storage account.

**Step 2**. Open the **Blobs** services and create a storage container.

**Step 3**. Under **Blob service** click *Soft delete*.  
![Blob Service options](/assets/images/screenshots/screenshot13.png)

**Step 4**. Turn on the *Soft delete* feature.  
![Soft delete](/assets/images/screenshots/screenshot14.png)

**Note**: There is a retention policies that indicates the period of time that soft deleted data could be stored and available for recovery from 1 day to maximum 365 days. Also, when you create new storage account the *soft delete* feature is off by default.

**Step 5**. Leave the default retention policy to 7 days, and click **Save**.

**Step 6**. Go to the blob container and upload a test file.

## Test *Soft delete* feature

**Step 7**. Select the file and remove it.
![Remove blob object](/assets/images/screenshots/screenshot15.png)

**Step 8**. Now check the *Show deleted blobs* option. It will list the deleted file with a status **Deleted**.
![List deleted blob object](/assets/images/screenshots/screenshot16.png)

**Step 9**. Click on the file. It will open a new window, click the **Undelete** button.
![List deleted blob object](/assets/images/screenshots/screenshot17.png)

**Step 10**. Go back to the blob container, it will list the file with status **Active**.
![List deleted blob object](/assets/images/screenshots/screenshot18.png)

## Conclusion
Enabling **Soft delete** will save you a lot of time, instead of recovering from backups. I think it's a nice feature to have in development environment. And for staging and production environments, make sure you configure the proper data redundancy option and backup strategy.

Official documentation: [Soft delete for Azure Storage Blobs](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-soft-delete){:target="_blank"}.