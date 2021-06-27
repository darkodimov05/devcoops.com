---
layout: post
title:  "How to backup and restore an Azure SQL database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-sql-database-backup-restore.jpg
tags: [ azure, azure-cli, azure sql database ]
---
You cannot think of a SQL Server Administration or any kind of a relational and non-relational database administration without the two magic words: **backup** and **restore**. Backing up a database is a part of an essential operation that can prevent disaster scenarios, like accidental corruption or deletion, and it's often underlooked when working in a development or staging environment. If you are working as a DevOps Cloud Architect, a part of your responsibilities you'll have to came up with a good backup and restore plan. This includes having backups on a cloud storage service, for example: Amazon S3, Azure Blob storage. You might also want to add a cold storage solution for long-term retention like Amazon Glacier and Azure Cool Storage, and most likely an on-premises storage solution.  
In this post, i will focus on how to backup and restore an Azure SQL Database using the Azure CLI, as a part of the Azure SQL Database Administration series.

## Azure SQL Database backups
Azure SQL Database automatically create database backups that can be kept from 7 up to 35 days, which is called a Backup retention period. Azure uses [RA-GRS](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy-grs#read-access-geo-redundant-storage){:target="_blank"} type of storage blobs to ensure redundancy in case if a primary region goes down. There is also a long-term retention policy for storing backups up to 10 years on a single database and elastic pools.  
Full backups are made every week, differential backups every 12 hours, and the most important ones, the transaction log backups are made every 5-10 minutes.

## Azure SQL Database restore
You can use the backups to restore a database to a point-in-time, to the time it was deleted, or from a specific long-term backup, if only the database has been configured with a long-term retention policy, all within the retention period. Also, you can create a new database to another geographical location from a backup.

More about [restoring dbs from backups](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-recovery-using-backups){:target="_blank"}.


## Prerequisites
* Azure account
* Azure SQL database
* Azure Storage account

## Backup an Azure SQL Database
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. There are two ways to export a database, using an SAS key or using a storage account key. I have already covered both in [How to manage blobs using Azure CLI]({% post_url 2019-09-30-how-to-manage-blobs-using-azure-cli %}){:target="_blank"}. I'll use a storage account key just for the purpose of this demo. Let's store the storage account key using a variable named **accountKey**, because we'll need it for later:  
```bash
accountKey=`az storage account show-connection-string --name "devcoopsdbaccount" --resource-group "db-backups-storage-rg" --query connectionString --output tsv | grep -oP 'AccountKey=\K.+'`
```
Parameters:
* `--name`: storage account name.
* `--resource-group`: resource group where the storage account belongs to.
* `--query`: filter the result to show the connectionString only.
* `--output`: output format.

**Step 3**. Create a backup file name using a timestamp using a variable named *dbbackupFileName*. For example:  
```bash
now=$(date -u +"%Y-%m-%d-%H-%M")
dbbackupFileName"dbbackup-$now
```

**Step 4**. Backup the database:  
```bash
az sql db export --server "devcoopsSQLserver" --name "dbName" --resource-group "db-backups-storage-rg" --admin-user "admin" --admin-password "<insert_password_here>" --storage-key-type StorageAccessKey --storage-key $accountKey --storage-uri "https://devcoopsdbaccount.blob.core.windows.net/db-backups/$dbbackupFileName.bacpac"
```
Parameters:
* `--server`: SQL server name.
* `--resource-group`: resource group where the database belongs to.
* `--admin-user`: admin user that will run the backup.
* `--admin-password`: admin user password.
* `--storage-key-type`: type of the storage key. It could be *SharedAccessKey* or *StorageAccountKey*.
* `--storage-key`: the Storage Key value.
* `--storage-uri`: the full path for the database backup file.


## Restore an Azure SQL database
**Step 5**. Import the database using the command:  
```bash
az sql db import --server "devcoopsSQLserver" --name "dbName" --resource-group "db-backups-storage-rg" --admin-user "admin" --admin-password "<insert_password_here>" --storage-key-type StorageAccessKey --storage-key $accountKey --storage-uri "https://devcoopsdbaccount.blob.core.windows.net/db-backups/$dbbackupFileName.bacpac"
```
Parameters:
* `--server`: SQL server name.
* `--resource-group`: resource group where the database belongs to.
* `--admin-user`: admin user that will run the backup.
* `--admin-password`: admin user password.
* `--storage-key-type`: type of the storage key. It could be *SharedAccessKey* or *StorageAccountKey*.
* `--storage-key`: the Storage Key value.
* `--storage-uri`: the backup full path from where an existing database will be restored.


More about [az sql db command](https://docs.microsoft.com/en-us/cli/azure/sql/db?view=azure-cli-latest){:target="_blank"}.

## Conclusion
In this post i have described a plain example on how you can backup and restore a database from the Azure CLI. It can be quite useful in situations when you immediately need a backup. In a real world scenario, always use automation which is manageable using the Azure Portal, PowerShell or REST API.