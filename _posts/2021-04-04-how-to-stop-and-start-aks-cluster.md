---
layout: post
title:  "How to stop and start Kubernetes cluster on Azure"
categories: [ azure, kubernetes ]
image: assets/images/azure-aks-start-stop.jpeg
tags: [ azure, azure-cli, kubernetes ]
---
In the previous Azure [post]({% post_url 2021-03-14-how-to-create-kubernetes-cluster-on-azure %}){:target="_blank"}, we've shown how to create an Azure Kubernetes Service (AKS) cluster via the Azure CLI. Today, we'll focus on how to stop and start AKS cluster using the official way. What the official way means anyway, you could ask. Well, before now, in order to save some infrastructure money, you need to scale down the User Node Pools to 0, but at at the end of each month, you'll still be charged for the system node pools which was running 24/7, unless you delete the AKS cluster. And, in the last couple of months, Azure came up with a feature that can stop the control plane (the system node pool) and the agent nodes as well.


## Prerequisites
* Azure account  
* Azure CLI  
* Azure Kubernetes Cluster

{% include autoads.html %}

## Stop the AKS Cluster
**Step 1**. Stop the cluster:    
```bash
az aks stop --resource-group rg-aks-test --name aksdevcoopstest
```

**Step 2**. Check the cluster status:    
```bash
az aks show --resource-group rg-aks-test --name aksdevcoopstest
```  
You should check the **powerState** value as a root element in the output JSON. **powerState** it's also part of **agentPoolProfiles** as well:  
```bash
"powerState": {
    "code": "Running"
},
```

## Start the AKS Cluster
**Step 3**. Start the cluster:    
```bash
az aks start --resource-group rg-aks-test --name aksdevcoopstest
```

**Step 4**. Check the cluster status:    
```bash
az aks show --resource-group rg-aks-test --name aksdevcoopstest
```  
Command output:  
```bash
"powerState": {
    "code": "Running"
},
```

## Cleanup
**Step 5**. Delete the Azure Kubernetes Cluster:  
```bash
az aks delete --resource-group "rg-aks-test" --name "aksdevcoopstest" --yes --no-wait
```

**Step 6**. Delete the resource group:  
```bash
az group delete --name "rg-aks-test"
```

## Conclusion
This is an awesome way to save some money, especially if you running an AKS cluster in dev or staging/preprod environments. The next step should be automation, setting up a scheduled task that will start the cluster early in the morning as you are getting your first cup of coffee and stopping the cluster after working hours. I'll leave this as a future post idea.  
More about [Stop and Start an Azure Kubernetes Cluster (AKS) cluster](https://docs.microsoft.com/en-us/azure/aks/start-stop-cluster){:target="_blank"}.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.
