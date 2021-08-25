---
layout: post
title:  "How to create Kubernetes cluster on Azure"
categories: [ azure, kubernetes ]
image: assets/images/azure-aks.jpeg
tags: [ azure, azure-cli, aks, kubernetes ]
---
Azure Kubernetes Service (AKS) is a managed Kubernetes service that helps you to deploy and manage clusters with ease. This post describes the commands in steps required to setup a Kubernetes cluster from the command line using the Azure CLI. 

## Prerequisites
* Azure account  
* Azure CLI  

## Create an Azure Kubernetes Service
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "rg-aks-test" --location westeurope
```

**Step 3**. Create a Kubernetes cluster in the resource group that we've just created:  
```bash
az aks create --resource-group "rg-aks-test" --name "aksdevcoopstest" --node-count 1 --generate-ssh-keys --node-vm-size Standard_D2s_v3
```
Parameters:
* `--resource-group`: name of the resource group.
* `--name`: name of the managed Kubernetes cluster.
* `--node-count`: number of nodes you want in your Kubernetes cluster.
* `--generate-ssh-keys`: generate SSH public and private keys files (if not specified) stored in the ~/ssh directory.
* `--node-vm-size`: size of VMs you want to use as Kubernetes nodes.

**Step 4**. Install the Kubernetes CLI. There are 2 ways to do this:  
1. Install [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/)
2. ```az aks install-cli```  

**Step 5**. Configure kubectl to connect to the Kubernetes cluster:  
```bash
az aks get-credentials --resource-group "rg-aks-test" --name "aksdevcoopstest" 
```

**Step 6**. Verify the connection to the cluster:
```bash
kubectl get nodes
```

## Cleanup
**Step 7**. Delete the Azure Kubernetes Cluster:  
```bash
az aks delete --resource-group "rg-aks-test" --name "aksdevcoopstest" --yes --no-wait
```

**Step 8**. Delete the resource group:  
```bash
az group delete --name "rg-aks-test"
```

## Conclusion
If Azure is your cloud of choice, then Azure Kubernetes Service provides the most easiest and secure way to deploy and manage a Kubernetes cluster. AKS has a CI/CD integration with [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/){:target="_blank"}, and the rest of the Azure services like [ACR]({% post_url 2021-03-07-push-docker-images-to-azure-container-registry %}){:target="_blank"} as container registry and [Azure Key Vault](https://azure.microsoft.com/en-us/services/key-vault/){:target="_blank"} for storing and managing secrets.