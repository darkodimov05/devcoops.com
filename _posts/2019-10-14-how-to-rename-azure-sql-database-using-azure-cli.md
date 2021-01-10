---
layout: post
title:  "How to rename an Azure SQL database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-sql-database-rename.jpg
tags: [ azure, azure-cli, azure sql database ]
---
As a part of an Azure SQL Database administration series, this post will be a follow up from the last [one]({% post_url 2019-10-13-how-to-recover-lost-admin-password-for-azure-sql-database-using-azure-cli %}){:target="_blank"}, where i will write about renaming Azure SQL database, which is kind of a common scenario for those who work as a DevOps / SysOps cloud engineer.  
There are currently three ways to rename an Azure SQL database, as far as i know.  
1. The first one is using TSQL, that requires **SQL Server Management Studio**. Run the following query:  
```
ALTER DATABASE [dbname] MODIFY NAME = [newDBname]
```
2. Rename using the **SQL Server Management Studio** GUI. Just right click on the database name and click *Rename*.
3. Rename using the Azure CLI, which will be explained in details in the next steps.


## Prerequisites
* Azure account
* Azure SQL database

{% include in-article-ad.html %}

## Rename an Azure SQL database
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Rename the database using the following command:  
```bash
az sql db rename --name "myfirstazuresqldb" --new-name "new-database-name" --resource-group "azure-sql-db-rg" --server "devcoopsdbserver"
```
Parameters:
* `--name`: database name before renaming.
* `--new-name`: new database name.
* `--resource-group`: name of the database resource group.
* `--server`: database server name.

Official documentation: [az sql db rename](https://docs.microsoft.com/en-us/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-rename){:target="_blank"}.

## Conclusion
My two cents on this is to write the command in a script that will accept input parameter, so you can call a bash script, for example:
```bash
./rename-az-sql-db.sh --name "new-name-db"
```