---
layout: post
title:  "Manage Azure subscriptions"
categories: [ azure ]
image: assets/images/azure-cloud-shell-subscriptions.png
tags: [azure, azure-cli]
---
Azure subscriptions are used for deploying and consuming Azure resources. A single Azure account can have multiple subscriptions, but there are some limitations. You can combine multiple Azure resources into a single Azure Resource Group. These groups are managed by [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).  
According to Microsoft, there are two *Subscription limits* tables: **Subscription limits** and **Subscription limits - Azure Resource Manager**.  
You can check the tables on the following link: [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits).  


## Prerequisites
* Azure account  
* Azure CLI  

{% include in-article-ad.html %}

## Configuration
**Step 1**. Open *Terminal* and list the active subscriptions:  
```bash
az account list --output table
```  
It will list all available subscriptions in a table format. You can output the results with other formats like: json, jsonc, yaml, and tsv.  

**Step 2**. Run a command to change the subscription.  
```bash
az account set --subscription <SubscriptionId>
```  

Replace the <SubscriptionId> part with the actual subscription id.  

## Check default subscription

It's always a good practice to check which subscription we are using before managing the Azure resources. There are a couple of ways to do this:

**1**. With *az account show* command:  
```bash
az account show --output jsonc
```  
![Terminal AZ Subscription](/assets/images/screenshots/screenshot4.png)

**When using *show* command, it will display the current subscription with every output format except table.**  

**2**. With *az account list* command:  
```bash
az account list --output table
```  
![Terminal AZ Subscription1](/assets/images/screenshots/screenshot7.png)  

Official documentation:  [Use multiple Azure subscriptions](https://docs.microsoft.com/en-us/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).