---
layout: post
title:  "How to create an Azure MySQL database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-mysql-database.jpg
tags: [ azure, azure-cli, mysql ]
---
MySQL is one of the most used open source relation database management system (RDBMS) with a client-server model. It's most commonly used as a web database, although it can have many other purposes, like data warehousing, e-commerce, and logging applications. If you have ever started working as a System Administrator, Database Administrator, or DevOps engineer, one of first things you'll have to face is creation or configuration of a MySQL instance.  
In today's post, i will write about creating a MySQL database in the cloud, and i'll stick with Azure for now. Microsoft's Azure has a service called `Azure Database Service` that is a relation database service for MySQL, PostgreSQL and MariaDB. You can easily create a MySQL server using the Azure Portal, or the Azure CLI.

## Prerequisites
* Azure account

## Create an Azure MySQL Database
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:
```bash
az group create --name "mysql-rg" --location westeurope
```

**Step 3**. Create a MySQL server: 
```bash
az mysql server create --name "devcoopsmysqldbserver" --resource-group "mysql-rg" --location westeurope --admin-user "devcoops" --admin-password "<insert_password_here>" --sku-name "B_Gen5_1"
```
Parameters:
* `--name`: MySQL server name.
* `--resource-group`: resource group where the mysql database server belongs to.
* `--location`: filter the result to show the connectionString only.
* `--admin-user`: server admin username.
* `--admin-password`: server admin password.
* `--sku-name`: SKU name in the following format: {pricing tier}_{compute generation}_{vCores}. More about the MySQL [pricing tiers](https://docs.microsoft.com/en-us/azure/mysql/concepts-pricing-tiers){:target="_blank"}.

**Step 4**. Get the connection information:  
```bash
az mysql server show --name "devcoopsmysqldbserver" --resource-group "mysql-rg"
```
The JSON output should look for the **administratorLogin** and **fullyQualifiedDomainName** parameters. For example:  
![Azure Database MySQL connection string](/assets/images/screenshots/screenshot31.png)

**Note**: Now that we have the connection information, we could create a database either by:
1. Connect to the server using the mysql command-line client tool, or the MySQL Workbench GUI tool and create a database:
```console
CREATE DATABASE mydb;
```
2. Using the Azure CLI.

**Step 5**. Create a Mysql database using Azure CLI:
```bash
az mysql db create --name "mydb" --resource-group "mysql-rg" --server-name "devcoopsmysqldbserver" 
```
Parameters:
* `--name`: MySQL database name.
* `--resource-group`: resource group where the database belongs to.
* `--server`: MySQL server name.

**Step 6**. Test the connection:  
```bash
mysql -h devcoopsmysqldbserver.mysql.database.azure.com -u devcoops@devcoopsmysqldbserver -p 
```

## Cleanup
**Step 7**. Delete the MySQL database:  
```bash
az mysql db delete --name "mydb" --resource-group "mysql-rg" --server-name "devcoopsmysqldbserver"
```

**Step 8**. Delete the MySQL server:  
```bash
az mysql server delete --name "mydb" --resource-group "mysql-rg"
```

**Step 9**. Delete the resource group:  
```bash
az group delete --name "mysql-rg"
```

More about [Azure Database for MySQL using Azure CLI](https://docs.microsoft.com/en-us/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli){:target="_blank"}.

## Conclusion
This is just a plain example, that shows how easily you can create a MySQL server and database in no time. It can be quite useful for development. If you have trouble connecting to the MySQL server, please check the following:
1. Configure firewall rules using the command:  
```bash
az mysql server firewall-rule create  --server "devcoopsmysqldbserver"  --resource-group "mysql-rg" --name AllowMyIP --start-ip-address "<insert_start_ip_here>" --end-ip-address "<insert_end_ip_here>"
```
2. Disable enforcing SSL (it's enabled by default on the MySQL server).
```bash
az mysql server update  --name "devcoopsmysqldbserver" --resource-group "mysql-rg" --ssl-enforcement Disabled
```
3. Connection string typo.