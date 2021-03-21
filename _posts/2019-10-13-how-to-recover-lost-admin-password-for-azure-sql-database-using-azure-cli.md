---
layout: post
title:  "How to recover lost admin password for an Azure SQL database using Azure CLI"
categories: [ azure ]
image: assets/images/azure-sql-database-password.jpg
tags: [ azure, azure-cli, azure sql database ]
---
A very common task that i'm used to as a DevOps engineer working in the cloud, is resetting forgotten or lost passwords for server or database access. Although one of the best practices is to store passwords in a key management service, like Azure Key Vault, that i have covered in this [post]({% post_url 2019-10-07-how-to-create-azure-key-vault-using-azure-cli %}){:target="_blank"}, people doesn't always follow security best practices. But, there could be another common scenario where you'll have to change the password every 3 or 6 months as a part of a password policy, for example the Azure SQL database Administrator password. There is a way to change the password using the Azure Portal that i'll cover in a future, but today i'll show you how to change the password quickly and easier from the Azure CLI, as a follow up from the ["How to create and configure Azure SQL Database using Azure CLI"](2019-10-12-how-to-create-and-configure-azure-sql-database-using-azure-cli){:target="_blank"} post.


## Prerequisites
* Azure account
* Azure SQL database

## Reset Administrator account password
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Type the following command:  
```bash
az sql server update --name "devcoopsdbserver" --resource-group "azure-sql-db-rg" --admin-password "<insert_new_admin_password_here>"
```
Parameters:
* `--name`: name of the database server.
* `--resource-group`: name of the database server resource group.
* `--admin-password`: new password for the database admin user.

**Note**: You could write the command in a script and run it as a cronjob.

## Conclusion
Periodically resetting the Azure SQL Administrator account password could lead us to an automation scenario. This can be quite useful when there is a password policy that apply to the Administrator account. Always use automation when possible.