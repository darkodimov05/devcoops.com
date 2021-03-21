---
layout: post
title:  "How to quickly create an Azure VM backup using the Azure CLI"
categories: [ azure ]
image: assets/images/azure-backup-vm.jpg
tags: [ azure, azure-cli, azure vm ]
---
In the last [post]({% post_url 2019-11-01-how-to-quickly-create-a-backup-of-an-azure-vm-using-the-azure-cli %}){:target="_blank"}, i was talking about taking Azure Virtual Machines disk snapshots, and why it's not a good idea to replace backups with snapshots. For today's post, i'll write about how to easily and quickly take backups from the `Azure Backup` service, using the Azure CLI.

## Prerequisites
* Azure account
* Azure VM

## Create a backup vault
**Step 1**. Open *Terminal* and sign in:  
```bash
az login
```  
It will open a new window using the default browser, where you will be prompted for email and password.

**Step 2**. First, we need a vault, where we'll be storing the backups. Create a vault using the command:  
```bash
az backup vault create --name "azure-vm-backup-vault" --resource-group "azure-vms-rg" --location westeurope
```
Example json output:  
![Azure backup vault](/assets/images/screenshots/screenshot45.png)

**Step 3**. Confirm the vault's creation by listing vaults in the resource group:  
```bash
az backup vault list --resource-group "azure-vms-rg" --output table
```
![Azure backup vault](/assets/images/screenshots/screenshot46.png)

## Enable and take a backup
**Step 4**. Enable Azure VM's backup:  
```bash
az backup protection enable-for-vm --policy-name "DefaultPolicy" --vault-name "azure-vm-backup-vault" --resource-group "azure-vms-rg" --vm "<azure_vm_name>"
```
Parameters:
* `--policy-name`: DefaultPolicy name.
* `--vault-name`: name of the backup vault.
* `--resource-group`: vault's resource group.
* `--vm`: VM's name, or ID. You could use the VM's name **only if** the VM and vault are in the same resource group.  
Check the *status* property in the json output:  
![Azure backup vault](/assets/images/screenshots/screenshot47.png)

**Step 5**. Now, take a backup:  
```bash
az backup protection backup-now --vault-name "azure-vm-backup-vault" --container-name "web-server-1"  --item-name "web-server-1" --resource-group "azure-vms-rg" --retain-until 31-12-2019
```
Parameters:
* `--vault-name`: backup vault's name.
* `--container-name`: VM name.
* `--item-name`: VM name.
* `--resource-group`: vault's resource group.
* `--retain-until`: last available date for the backup.

**Step 6**. Check the backup status:  
```bash
az backup job list --vault-name "azure-vm-backup-vault" --resource-group "azure-vms-rg" --output table
```
Example json output:  
![Azure backup show-status](/assets/images/screenshots/screenshot48.png)

## Cleanup
**Step 7**. Disable the backup protection:  
```bash
az backup protection disable --vault-name "azure-vm-backup-vault" --container-name "web-server-1" --item-name "web-server-1" --resource-group "azure-vms-rg" --delete-backup-data true
```

**Step 8**. Remove the backup vault:  
```bash
az backup vault delete --name "azure-vm-backup-vault" --resource-group "azure-vms-rg"
```

**Step 9**. Remove the resource group:  
```bash
az group delete --name "azure-vms-rg"
```

Official documentation [Back up a virtual machine in Azure with the CLI](https://docs.microsoft.com/en-us/azure/backup/quick-backup-vm-cli){:target="_blank"}.