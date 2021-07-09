---
layout: post
title:  "How to create and configure Azure SQL Database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-sql-database.jpg
tags: [ azure, azure-cli, azure sql database ]
---
Azure SQL Database is an Azure cloud managed service that provides relational database-as-a-service (DBaaS). In other words, you can migrate or scale on-premise SQL databases without worrying about high availability, backups, patching updates, or performance, because Azure is responsible for all of these operation tasks including the underlying infrastructure. You'll be charged only for the database server and for each user-created databases.  
There are currently three deployment models for an Azure SQL database:  
![Azure SQL Database deployment-models](/assets/images/screenshots/screenshot29.png)

* **Single database** -- *Single database* is a fully managed database that is most commonly used for dev environment purposes, or if you need a single data source.
* **Managed instance** -- *Managed instance* is a fully managed SQL Server Database Engine instance, that could be used when migrating on-premises database to the Azure.
* **Elastic pool** -- *Elastic pool* is a collection of single databases that are stored on a single Azure SQL Database server and use shared resources. It represent a cost-effective solution when hosting multiple databases in the cloud.

Check the official documentation [Azure SQL Database documentation](https://docs.microsoft.com/en-us/azure/sql-database/){:target="_blank"}.

## Prerequisites
* Azure account

## Create and configure an Azure SQL database server
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "azure-sql-db-rg" --location westeurope
```

**Step 3**. Create a database server for the hosted database:  
```bash
az sql server create --name "devcoopsdbserver" --resouce-group "azure-sql-db-rg" --location westeurope --admin-user "devcoops" --admin-password "<insert-admin-password>"
```
Parameters:
* `--resource-group`: name of the resource group.
* `--location`: location of the database server.
* `--admin-user`: name of the database admin user.
* `--admin-password`: password of the database admin user.

**Step 4**. Now that you have created a database server, configure firewall rule to whitelist IP addresses that can have access to the server. For example:   
```bash
az sql server firewall-rule create --server "devcoopsdbserver" --resource-group "azure-sql-db-rg" --name "WhitelistIPs" --start-ip-address "10.10.10.10" --end-ip-address "10.10.10.255"
```
Parameters:
* `--server`: name of the database server.
* `--resource-group`: name of the resource group.
* `--name`: name of the firewall rule.
* `--start-ip-address`: start of the ip address range to allow access to the database server.
* `--end-ip-address`: end of the ip address range to allow access to the database server.

**Note**: Computers that have IP addresses in the following range from 10.10.10.10 to 10.10.10.255 are allowed to access the database server.

## Create an Azure SQL database
**Step 5**. Create an Azure SQL database:  
```bash
az sql db create --server "devcoopsdbserver" --resource-group "azure-sql-db-rg" --name "myfirstazuresqldb" --edition "GeneralPurpose" --family "Gen4" --capacity 1 --zone-redundant false
```
Parameters:
* `--server`: name of the database server.
* `--resource-group`: name of the resource group.
* `--name`: name of the database.
* `--edition`: edition of the SKU.
* `--family`: the compute generation of the SKU: Gen4 or Gen5.
* `--capacity`: number of vcores.
* `--zone-redundant`: enable or disable replication of your data across zones in a single region.

**Note**: Once you have created the database, you can check the resources created in the Azure Portal, by clicking on the "azure-sql-db-rg" resource in the *Resource groups* tab. It should look something like this:  
 ![Azure SQL Database resource-group](/assets/images/screenshots/screenshot30.png)

## Cleaning up
**Step 6**. Delete the Azure SQL database:  
```bash
az sql db delete --name "devcoopsazuresqldb" --server "devcoopsdbserver" --resource-group "azure-sql-db-rg"
```

**Step 7**. Delete the database server:  
```bash
az sql server delete --name "devcoopsdbserver" --resource-group "azure-sql-db-rg"
```

**Step 8**. Delete the resource group:  
```bash
az group delete --name "azure-sql-db-rg"
```

## Conclusion
If you are already using Azure ecosystem, migrating your on-premises SQL database to the Azure will improve the availability and performance while interacting with the other Azure services.  
In the next posts, i'll show you additional Azure SQL database operations, like how to lost a recover admin password, rename a database, backup and restore, and how to migrate your on-premise SQL database to the Azure cloud.