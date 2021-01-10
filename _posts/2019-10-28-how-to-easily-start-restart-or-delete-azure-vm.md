---
layout: post
title:  "How to easily Start, Stop, Restart or Delete Azure VMs"
categories: [ azure ]
image: assets/images/azure-manage-vm.jpg
tags: [ azure, azure-cli ]
---
Managing Azure Virtual Machines through the Portal could be pretty straighforward. But, it requires multiple steps using the GUI. For example, you'll have to login to the Portal first, then go to the `Azure Virtual Machines` service, select the VMs, and then you can start, stop, restart, or even delete the virtual machines. There is also a quicker way to do these operations, especially if you are the SysOps / DevOps guy.  
Although, there are three other ways to manage Azure VMs, including PowerShell cmdlets, ARM templates, and the Azure CLI, today i'll focus on the CLI.  

## Prerequisites
* Azure account
* Azure VMs

{% include in-article-ad.html %}

## List Azure VM using the CLI
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser, where you will be prompted for email and password.

**Step 2**. Now, we need the list of Azure Virtual machines ids:   
```bash
az vm list --query "[].id" --output tsv
```
Parameters:
* `--query`: parameter to select only the IDs of all virtual machines.
* `--output`: output the results in a tab-seperated format.  

Example output:  
```json
/subscription/<SUBSCRIPTION_ID>/resourceGroups/AZURE-VMS-RG/providers/Microsoft.Compute/virtualMachines/web-server-1
/subscription/<SUBSCRIPTION_ID>/resourceGroups/AZURE-VMS-RG/providers/Microsoft.Compute/virtualMachines/web-server-2
```

## Start Azure VMs
**Step 3**. Start a single Azure VM instance:  
```bash
az vm start --resource-group "azure-vms-rg" --name "<name_of_vm_instance>"
```

**Step 4**. Start all Azure VM instances:    
```bash
az vm start --ids $(az vm list --resource-group "azure-vms-rg" --query "[].id" --output tsv)
```

## Stop Azure VMs
**Step 5**. Stop a single Azure VM instance:    
```bash
az vm stop --resource-group "azure-vms-rg" --name "<name_of_vm_instance>"
```

**Step 6**. Stop all Azure VM instances:  
```bash
az vm stop --ids $(az vm list --resource-group "azure-vms-rg" --query "[].id" --output tsv)
```

**Note**: Stopping a virtual machine will still generate costs, because you are holding on the VM's resources, like: CPU, Memory, Disk, and so on. You could save money by deallocation, using the `az vm deallocate` command. For example:  
**Step 7**. Deallocate a single VM:  
```bash
az vm deallocate --resource-group "azure-vms-rg" --name "<name_of_vm_instance>"
```

**Step 8**. Deallocate all VMs:  
```bash
az vm deallocate --ids $(az vm list --resource-group "azure-vms-rg" --query "[].id" --output tsv)
```

## Restart Azure VMs
**Step 9**. Restart a single Azure VM instance:     
```bash
az vm restart --resource-group "azure-vms-rg" --name "<name_of_vm_instance>"
```

**Step 10**. Restart all Azure VM instances:  
```bash
az vm restart --ids $(az vm list --resource-group "azure-vms-rg" --query "[].id" --output tsv)
```

## Delete Azure VMs
**Step 11**. Delete a single Azure VM instance:  
```bash
az vm delete --resource-group "azure-vms-rg" --name "<name_of_vm_instance>"
```

**Step 12**. Delete all Azure VM instances:  
```bash
az vm delete --ids $(az vm list --resource-group "azure-vms-rg" --query "[].id" --output tsv)
```


Also, there is a way, if you want to manage a couple of virtual machines, by filtering the results using the pipe character `|` and the `grep` command. For example, if i want to delete VMs that include the "dev" string, i'll execute something like this:  
```bash
az vm delete --ids $(az vm list --resource-group "azure-vms-rg" --query "[].id" --output tsv) | grep "dev"
```

It goes the same for start, stop, restart, and deallocate operations.


Official documentation [az vm](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest){:target="_blank"}.

## Conclusion
Managing `Azure Virtual Machines` through the Azure CLI could save you a lot of time, and you'll be more productive. But, you should always be careful when executing these commands, especially when running the **delete** command.