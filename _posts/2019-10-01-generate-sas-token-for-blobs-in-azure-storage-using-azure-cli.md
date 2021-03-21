---
layout: post
title:  "Generate SAS token for blobs in Azure storage using Azure CLI"
categories: [ azure ]
image: assets/images/azure-cli-sas-token-storage.jpg
tags: [azure, azure cli, azure storage]
---
A shared access signature (SAS) provides secure and temporary access to the resources in a storage account. You can configure access to specific objects, as well as permissions and SAS token validation time.

## SAS types
There are few types of SAS:
* **User delegation SAS**: User delegation SAS is secured with Azure Active Directory (Azure AD) credentials. It delegates access only to blob storage resources.
* **Service SAS**: A service SAS is secured with the storage account key. It delegates access to one of the following Azure storage services: Blob storage, Queue storage, Table storage, or Azure files. 
* **Account SAS**: An account SAS is secured with the storage account key. It delegates access to read, write and delete operations on azure storage resources, that are not permitted with a *service SAS*.

Regarding security, Microsoft recommends using **User delegation SAS** when possible.

## SAS forms
A SAS can take one of two forms:  
* **Ad hoc SAS**: This is the default SAS form. Any type of SAS can be an Ad hoc SAS. The start time, expiration time and permissions are all included in the SAS URI.  
* **Service SAS with stored access policy**: A stored access policy operates on a resource container level, which can be: blob container, table, queue or file share. The SAS inherits the start time, expiration time, and permissions defined in the stored access policy.

More about [Azure SAS](https://docs.microsoft.com/en-us/azure/storage/common/storage-sas-overview).

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
az storage blob generate-sas --account-name devcoopsstorage1 --container-name myfirstblobcontainer --name index.php --permissions acdrw --expiry 2019-10-02
```
Parameters:
* `--account-name`: name of the storage account.
* `--container-name`: name of the storage container.
* `--name`: name of the blob file.
* `--permissions`: 
    * a = Add
    * c = Create
    * d = Delete
    * r = Read
    * w = Write
* `--expiry`: datetime (Y-m-d'T'H:M'Z') at which SAS becomes invalid.

It will return SAS token:
```
"se=2019-10-02&sp=racwd&sv=2018-11-09&sr=b&sig=Co9T1DZwcbVNSPy4ACD2Lyjob0KlN3QBKyiIV6fQHjg%3D"
```

**Note**: SAS token is a string that you generate on the client side. You can create unlimited number of tokens, which also are not tracked by Azure Storage in any way.

**Step 3**. Run the same command with the *--full-uri* parameter:
```bash
az storage blob generate-sas --account-name devcoopsstorage1 --container-name myfirstblobcontainer --name index.php --permissions acdrw --expiry 2019-10-02 --full-uri
```
Parameters:
* `--full-uri`: It will return the SAS URI.

URL of the blob in the Azure Storage:
```
https://devcoopsstorage1.blob.core.windows.net/myfirstblobcontainer/index.php
```

Returned SAS URI:
```
"https://devcoopsstorage1.blob.core.windows.net/myfirstblobcontainer/index.php?se=2019-10-02&sp=racwd&sv=2018-11-09&sr=b&sig=Co9T1DZwcbVNSPy4ACD2Lyjob0KlN3QBKyiIV6fQHjg%3D"
```

As you can see, the SAS URI is created from two parts:
* The Storage Resource URI: `https://devcoopsstorage1.blob.core.windows.net/myfirstblobcontainer/index.php?`
* The SAS Token: `se=2019-10-02&sp=racwd&sv=2018-11-09&sr=b&sig=Co9T1DZwcbVNSPy4ACD2Lyjob0KlN3QBKyiIV6fQHjg%3D`

**Note**: When an application sends a SAS URI to Azure as part of a request, the Azure storage service checks the SAS parameters and signature to verify that it's valid. 

## Revoke a user delegation SAS

**Step 4**. Run the following command:
```bash
az storage account revoke-delegation-keys --name devcoopsstorage1 --resource-group storage-rg
```

## Next steps
We can try to grant limited access to storage containers as well using **user delegation SAS**.

Official documentation: [Create a user delegation SAS for a container or blob with the Azure CLI](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-user-delegation-sas-create-cli).