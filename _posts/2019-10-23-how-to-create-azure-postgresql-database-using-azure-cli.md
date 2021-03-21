---
layout: post
title:  "How to create an Azure PostgreSQL database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-postgresql-database.jpg
tags: [ azure, azure-cli, postgresql ]
---
In the previous posts, i've covered the creation of MySQL and MariaDB using the `Azure Database` service. Today, i'll write about PostgreSQL, the last one Azure's Database service to offer.  
PostgreSQL is a free and open-source relational database management system. It's a general-purpose RDBMS, designed to be extensible, and got advanced features like the other enterprise database management systems.  
The PostgreSQL database creation procedure is almost the same as the MySQL and MariaDB.  

## Prerequisites
* Azure account

## Create an Azure PostgreSQL Database
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:
```bash
az group create --name "postgresql-rg" --location westeurope
```

**Step 3**. Create a PostgreSQL server: 
```bash
az postgres server create --name "devcoopsdbserver" --resource-group "postgresql-rg" --location westeurope --admin-user "devcoops" --admin-password "<insert_password_here>" --sku-name "B_Gen5_1"
```
Parameters:
* `--name`: PostgreSQL server name.
* `--resource-group`: resource group where the postgresql database server belongs to.
* `--location`: filter the result to show the connectionString only.
* `--admin-user`: server admin username.
* `--admin-password`: server admin password.
* `--sku-name`: SKU name in the following format: {pricing tier}_{compute generation}_{vCores}. More about the MariaDB [pricing tiers](https://docs.microsoft.com/en-us/azure/postgresql/concepts-pricing-tiers){:target="_blank"}.

**Step 4**. Get the connection information:  
```bash
az postgres server show --name "devcoopsdbserver" --resource-group "postgresql-rg"
```
The JSON output should look for the **administratorLogin** and **fullyQualifiedDomainName** parameters. For example:  
![Azure Database PostgreSQL connection string](/assets/images/screenshots/screenshot33.png)

**Note**: Now that we have the connection information, we could create a database either by:
1. Connect to the server using the PostgreSQL cli, or the pgAdmin GUI tool and create a database:
```console
CREATE DATABASE mydb;
```
2. Using the Azure CLI.

**Step 5**. Create a PostgreSQL database using Azure CLI:
```bash
az postgres db create --name "mydb" --resource-group "postgresql-rg" --server-name "devcoopsdbserver" 
```
Parameters:
* `--name`: PostgreSQL database name.
* `--resource-group`: resource group where the database belongs to.
* `--server`: PostgreSQL server name.

**Step 6**. Test the connection:  
```bash
psql --host=devcoopsdbserver.postgres.database.azure.com --port=5432 --username=devcoops@devcoopsdbserver --dbname=mydb 
```

## Cleanup
**Step 7**. Delete the PostgreSQL database:  
```bash
az postgres db delete --name "mydb" --resource-group "postgresql-rg" --server-name "devcoopsdbserver"
```

**Step 8**. Delete the PostgreSQL server:  
```bash
az postgres server delete --name "mydb" --resource-group "postgresql-rg"
```

**Step 9**. Delete the resource group:  
```bash
az group delete --name "postgresql-rg"
```

More about [Azure Database for PostgreSQL using Azure CLI](https://docs.microsoft.com/en-us/azure/postgresql/quickstart-create-server-database-azure-cli){:target="_blank"}.

## Conclusion
This is just a plain example, that shows how easily you can create a PostgreSQL server and database in no time. It can be quite useful for development. If you have trouble connecting to the PostgreSQL server, please check the following:
1. Configure firewall rules using the command:  
```bash
az postgres server firewall-rule create  --server "devcoopsdbserver"  --resource-group "postgresql-rg" --name AllowMyIP --start-ip-address "<insert_start_ip_here>" --end-ip-address "<insert_end_ip_here>"
```
2. Disable enforcing SSL (it's enabled by default on the PostgreSQL server).
```bash
az postgres server update  --name "devcoopsbdbserver" --resource-group "postgresql-rg" --ssl-enforcement Disabled
```
3. Connection string typo.