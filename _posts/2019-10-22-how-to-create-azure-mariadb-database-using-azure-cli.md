---
layout: post
title:  "How to create an Azure MariaDB database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-mariadb-database.jpg
tags: [ azure, azure-cli, mariadb ]
---
Besides MySQL, MariaDB is another free open source relation database under the GNU General Public License, that is often compared to MySQL. That's because MariaDB is a fork of the MySQL RDBMS, which means it's based on the same source code, and usually it's used for the same purposes, as a web database.  
In today's post, i'll continue explore the possibilities of creating a relation database using the `Azure SQL Database` service, using the Azure CLI. In the last post, i've explained the simplified steps on [How to backup and restore an Azure SQL database using Azure CLI]({% post_url 2019-10-18-how-to-create-azure-mysql-database-using-azure-cli %}){:target="_blank"}, so if you have already seen it, it's pretty much the same procedure under the Azure Cloud.

## Prerequisites
* Azure account

## Create an Azure MariaDB Database
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:
```bash
az group create --name "mariadb-rg" --location westeurope
```

**Step 3**. Create a MariaDB server: 
```bash
az mariadb server create --name "devcoopsmariadbserver" --resource-group "mariadb-rg" --location westeurope --admin-user "devcoops" --admin-password "<insert_password_here>" --sku-name "B_Gen5_1"
```
Parameters:
* `--name`: MariaDB server name.
* `--resource-group`: resource group where the mariadb database server belongs to.
* `--location`: filter the result to show the connectionString only.
* `--admin-user`: server admin username.
* `--admin-password`: server admin password.
* `--sku-name`: SKU name in the following format: {pricing tier}_{compute generation}_{vCores}. More about the MariaDB [pricing tiers](https://docs.microsoft.com/en-us/azure/mariadb/concepts-pricing-tiers){:target="_blank"}.

**Step 4**. Get the connection information:  
```bash
az mariadb server show --name "devcoopsmariadbserver" --resource-group "mariadb-rg"
```
The JSON output should look for the **administratorLogin** and **fullyQualifiedDomainName** parameters. For example:  
![Azure Database MariaDB connection string](/assets/images/screenshots/screenshot32.png)

**Note**: Now that we have the connection information, we could create a database either by:
1. Connect to the server using the mysql command-line client tool, or the MySQL Workbench GUI tool and create a database:
```console
CREATE DATABASE mydb;
```
2. Using the Azure CLI.

**Step 5**. Create a MariaDB database using Azure CLI:
```bash
az mariadb db create --name "mydb" --resource-group "mariadb-rg" --server-name "devcoopsmariadbserver" 
```
Parameters:
* `--name`: MariaDB database name.
* `--resource-group`: resource group where the database belongs to.
* `--server`: MariaDB server name.

**Step 6**. Test the connection:  
```bash
mysql -h devcoopsmariadbserver.mariadb.database.azure.com -u devcoops@devcoopsmariadbserver -p 
```

## Cleanup
**Step 7**. Delete the MariaDB database:  
```bash
az mariadb db delete --name "mydb" --resource-group "mariadb-rg" --server-name "devcoopsmariadbserver"
```

**Step 8**. Delete the MariaDB server:  
```bash
az mariadb server delete --name "mydb" --resource-group "mariadb-rg"
```

**Step 9**. Delete the resource group:  
```bash
az group delete --name "mariadb-rg"
```

More about [Azure Database for MariaDB using Azure CLI](https://docs.microsoft.com/en-us/azure/mariadb/quickstart-create-mariadb-server-database-using-azure-cli){:target="_blank"}.

## Conclusion
This is just a plain example, that shows how easily you can create a MariaDB server and database in no time. It can be quite useful for development. If you have trouble connecting to the MariaDB server, please check the following:
1. Configure firewall rules using the command:  
```bash
az mariadb server firewall-rule create  --server "devcoopsmariadbserver"  --resource-group "maria-rg" --name AllowMyIP --start-ip-address "<insert_start_ip_here>" --end-ip-address "<insert_end_ip_here>"
```
2. Disable enforcing SSL (it's enabled by default on the MariaDB server).
```bash
az mariadb server update  --name "devcoopsmariadbdbserver" --resource-group "mariadb-rg" --ssl-enforcement Disabled
```
3. Connection string typo.