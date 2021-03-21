---
layout: post
title:  "Generate SAS token for Azure storage containers using Azure CLI"
categories: [ azure ]
image: assets/images/azure-cli-sas-token-storage.jpg
tags: [azure, azure cli, azure storage]
---
As a part of the [Azure](https://devcoops.com/categories#azure) series, today we are going to generate SAS token for the Azure storage containers using the azure CLI. It's pretty much the same steps as generating SAS token for a blob, but instead of using the [*azure storage blob generate-sas*](https://docs.microsoft.com/en-us/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-generate-sas), we are going to use [*azure storage container generate-sas*](https://docs.microsoft.com/en-us/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-generate-sas).

 Please see [Generate SAS token for blobs in Azure storage using Azure CLI]({% post_url 2019-10-01-generate-sas-token-for-blobs-in-azure-storage-using-azure-cli %}) in case you want to see how to generate sas token for blobs in more details.

## Prerequisites
* Azure account
* Azure CLI
* Azure Storage account

## Create a user delegation SAS for a blob
**Step 1**. Open *Terminal* and login to the Azure Portal:
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Run the following command:
```bash
az storage container generate-sas --account-name devcoopsstorage1 --name myfirstblobcontainer --permissions acdlrw --expiry 2019-10-03
```
Parameters:
* `--account-name`: name of the storage account.
* `--name`: name of the storage container.
* `--permissions`: 
    * a = Add
    * c = Create
    * d = Delete
    * l = List
    * r = Read
    * w = Write
* `--expiry`: datetime (Y-m-d'T'H:M'Z') at which SAS becomes invalid.

It will return SAS token:
```
"se=2019-10-03&sp=racwdl&sv=2018-11-09&sr=c&sig=MLOqEjIYDtltmvVQwa/sWDub8B6h8i1A4SGxqlyW8pM%3D"
```

**Note**: SAS token is a string that you generate on the client side. You can create unlimited number of tokens, which also are not tracked by Azure Storage in any way.

## Revoke a user delegation SAS

**Step 4**. Run the following command:
```bash
az storage account revoke-delegation-keys --name devcoopsstorage1 --resource-group storage-rg
```

**Note**: Azure Storage cache the user delegation key, so there could be a delay window between the initiation of the revocation process and the invalidation of the **user delegation SAS**.  

Official documentation: [Create a user delegation SAS for a container or blob with the Azure CLI](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-user-delegation-sas-create-cli).