---
layout: post
title:  "Push Docker Images to Azure Container Registry using Azure CLI"
categories: [ azure, docker ]
image: assets/images/azure-acr.jpeg
tags: [ azure, azure-cli, acr, docker ]
---
Azure Container Registry is a private container registry where you can upload and manage your Docker and Open Container Initiative (OCI) images. At the time of writing, the cost of the basic ACR tier including 10GiB storage is 5$ per month. 


## Prerequisites
* Azure account  
* Azure CLI  

## Create an Azure Container Registry
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "rg-container-registry-test-001" --location westeurope
```

**Step 3**. Create a container registry in the resource group that we've just created:  
```bash
az acr create --resource-group "rg-container-registry-test-001" --name "acrdevcoopstest001" --location westeurope --sku Basic --admin-enabled true
```
Parameters:
* `--resource-group`: name of the resource group.
* `--name`: name of the container registry.
* `--location`: location of the acr service.
* `--sku`: the SKU of the container registry.
* `--admin-enabled`: enable admin user for testing purposes only.

**Step 4**. Login to the Azure Container Registry:  
```bash
az acr login --name "acrdevcoopstest001"
```  
It should display message: ```Login Succeeded```

**Step 5**. The Docker Image needs to be tagged with the ACR's loginServer name, so let's query it:  
```bash
az acr list --resource-group "rg-container-registry-test-001" --query "[].{acrLoginServer:loginServer}" --output table
```
Output:  
```
AcrLoginServer
-----------------------------
acrdevcoopstest001.azurecr.io
```

**Step 6**. Now, tag your Docker Image:  
```bash
docker tag mytestimage acrdevcoopstest001.azurecr.io/mytestimage
```

**Step 7**. Push Docker Image to Azure Container Registry:  
```bash
docker push acrdevcoopstest001.azurecr.io/mytestimage
```

**Step 8**. List Docker Images in the registry:  
```bash
az acr list --resource-group "rg-container-registry-test-001" --output table
```
Output:  
```
| NAME               | RESOURCE GROUP                 | LOCATION   | SKU   | LOGIN SERVER                  | CREATION DATE        | ADMIN ENABLED |
| ------------------ | ------------------------------ | --------   | ----- | ----------------------------- | -------------------- | ------------- |
| acrdevcoopstest001 | rg-container-registry-test-001 | westeurope | Basic | acrdevcoopstest001.azurecr.io | 2021-03-07T15:11:37Z | True          |
 ```

## Cleanup
**Step 9**. Delete the Azure Container Registry:  
```bash
az acr delete --resource-group "rg-container-registry-test-001" --name "acrdevcoopstest001"
```

**Step 10**. Delete the resource group:  
```bash
az group delete --name "rg-container-registry-test-001"
```

## Conclusion
If you are working with container services on the Azure Cloud, then Azure Container Registry is the perfect solution for storing and managing container images.