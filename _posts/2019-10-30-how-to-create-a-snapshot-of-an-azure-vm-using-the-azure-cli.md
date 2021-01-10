---
layout: post
title:  "How to create a snapshot of an Azure VM using the Azure CLI"
categories: [ azure ]
image: assets/images/azure-snapshot-vm.jpg
tags: [ azure, azure-cli, azure vm ]
---
Snapshot is a popular term in the virtual storage world. Snapshots are a full, point-in-time copies of a system, mostly virtual machines disks, known as VHDs. We could use snapshots to restore a virtual machine to a stable state, for example if we broke something and we want to do a rollback, or even create a new copy of the VM in another availability zone in the cloud, by creating an image first.   
Managing snapshots in the Azure could be done by using the Portal, the PowerShell cmdlet, or the Azure CLI. Today, i'll write about how to create quick snapshots of an Azure Virtual Machine disk, using the `Azure CLI`.

## Prerequisites
* Azure account
* Azure VM

{% include in-article-ad.html %}

## Create a snapshot of an Azure VM disk
**Step 1**. Open *Terminal* and sign in:  
```bash
az login
```  
It will open a new window using the default browser, where you will be prompted for email and password.

**Step 2**. Find the ID of the virtual machine disk and store it into a variable:  
```bash
diskID=$(az vm show  --resource-group "azure-vms-rg" --name "web-server-1" --query "storageProfile.osDisk.managedDisk.id" | grep -oP 'disks/\K.+' | rev | cut -c2- | rev)
```
Parameters:
* `--resource-group`: the VM resource group.
* `--name`: VM name.
* `--query`: query the VM disk ID.

**Step 3**. Create a *date* variable, for the snapshot name:   
```bash
now=$(date -u +"%Y-%m-%d-%H-%M")
```

**Step 4**. Now, create a snapshot using the following command:  
```bash
az snapshot create --name "Snapshot_$now" --resource-group "azure-vms-rg" --source $diskID
```
Parameters:
* `--name`: snapshot name.
* `--resource-group`: resource group where the snapshot will be a part of.
* `--source`: VM's disk ID.

## Remove an Azure VM's snapshot
**Step 5**. List all snapshot in the resource group:  
```bash
az snapshot list --resource-group "azure-vms-rg"
```

**Step 6**. Delete the snapshot:  
```bash
az snapshot delete --ids "<snapshot_id>"
```


Official documentation [az snapshot](https://docs.microsoft.com/en-us/cli/azure/snapshot?view=azure-cli-latest){:target="_blank"}.

## Conclusion
As a best practise, automate the snapshot creations, and don't rely on snapshots only! Although snapshots could be used as a short-term backups, or ad-hoc backups, they cannot replace backups! Use [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-overview){:target="_blank"} as a backup feature, and `[Azure Site Recovery](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview){:target="_blank"} for disaster recovery scenarios, which i will cover both in the future.