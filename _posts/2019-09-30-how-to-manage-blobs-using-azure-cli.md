---
layout: post
title:  "How to manage blobs using Azure CLI"
categories: [ azure ]
image: assets/images/azure-cloud-shell-blobs.png
tags: [azure, azure-cli]
---
In the last Azure [post]({% post_url 2019-09-27-how-to-create-azure-storage-account-using-azure-cli %}) we have discussed the different types of Azure storage accounts and created one using the Azure CLI. Next, we are going to upload, download and list blob files named **Blobs**.  
**Blobs** is a REST-based object storage service that is used for storing large ammount of unstructured data, such as text, or binaries. The most common scenario for **Blob Storage** is serving static website using **CDN** (Content Delivery Network). Other examples may include:
* Storing media files
* Streaming
* Storing data for analysis
* Storing files for backups, disaster recovery, archiving or other purposes.

**Blobs** are comprised of many blocks, each identified by a *block ID*. There are 3 types of blobs:
* *block blobs*: Optimized for uploading larger blobs.
* *page blobs*: Optimized for random read and write operations. Maximum size for page blobs is 8 TB.
* *append blobs*: Optimized for append operations. Append blob can have arount 195 GB with up to 50000 blocks with up to 4 MB in size.

More about [Azure Blob types](https://docs.microsoft.com/en-us/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) and [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/).

## Prerequisites
* Azure account
* Azure CLI
* Azure storage account

{% include in-article-ad.html %}

## Configure storage account credentials
**Step 1**. Open *Terminal* and login to the Azure Portal:
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Before doing any operation on the storage account, we need the storage account credentials. There are several ways to provide these credentials, but the *easiest* and *not so secure one* is using *AZURE_STORAGE_ACCOUNT* and *AZURE_STORAGE_KEY* environment variables. List the storage account keys:
```bash
az storage account keys list --account-name devcoops1 --resource-group storage-rg --output table
```

The output should look like this:  
![Azure CLI storage account keys list](/assets/images/screenshots/screenshot9.png)

**Step 3**. Export the environment variables including a storage key listed in **Step 2**, or create a new key:
```bash
export AZURE_STORAGE_ACCOUNT="devcoops1"
export AZURE_STORAGE_KEY="<insert_a_storage_key_here>"
```

**Note**: Make sure the environment variables exports are correct by running **echo $AZURE_STORAGE_ACCOUNT** and **echo $AZURE_STORAGE_KEY**.

## Create a blob container

**Step 4**. Azure Blobs are always stored and grouped in containers. Think of them as folders or directories where files are stored. Create a blob container using the following command:
```bash
az storage container create --name myfirstblobcontainer
```
JSON response message:
```json
{ "created": true }
```

**Note**: If an error message pop-up: `Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known'))`, there are a few workarounds:
1. Add ```0.0.0.0 localhost``` line to the *hosts* file. Don't forget to remove the line once you are done with the example.
2. Connect the Azure Cloud Shell to the Visual Studio Code using the instructions on this [post](https://devcoops.com/add-azure-cloud-shell-to-vscode/).
3. Open the Azure Cloud Shell from the Azure Portal.

## Upload a blob

**Step 5**. First, we are going to create a file before the upload to a blob. Create a sample "Hello World" file *index.php*:
```bash
echo "<?php echo "Hello World\!";?>" > index.php
```

**Step 6**. Next, upload the file:
```bash
az storage blob upload --container-name myfirstblobcontainer --name helloworld --file index.php
```

## List the blob file

**Step 7**. Now, we are going to confirm the file upload by listing the container:
```bash
az storage blob list --container-name myfirstblobcontainer --output table
```
Output:
![Azure CLI list blob container](/assets/images/screenshots/screenshot10.png)

## Download the blob file

**Step 8**. You could also download the blob file by using the command:
```bash
az storage blob download --container-name myfirstblobcontainer --name helloworld --file index.php
```

## Cleanup

If you aren't using the storage account anymore, you could delete the resource group anytime using the following command:
```bash
az group delete --name storage-rg
```

## Next steps
Instead of using storage access key, we'll configure a shared access signature known as SAS token.

Official documentation: [Upload, download, and list blobs using the Azure CLI](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-cli).