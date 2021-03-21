---
layout: post
title:  "How to create and manage public Azure DNS using Azure CLI"
categories: [ azure ]
image: assets/images/azure-dns.jpg
tags: [ azure, azure-cli ]
---
Azure DNS is a cloud-based DNS hosting service that allows you to host and manage your own domains. These domains are hosted on Azure's global network of DNS name servers. Keep in mind that you can't buy a domain name from Azure DNS, instead you can use the [App Service domains](https://docs.microsoft.com/en-gb/azure/app-service/manage-custom-dns-buy-domain#buy-the-domain){:target="_blank"}, or a third-party domain registrar like: GoDaddy, Namecheap, AWS Route53, Google Cloud DNS, and so on. If you already an owner of a public DNS zone, you could just configure the name servers NS records to point to those of Azure. So, why bother hosting on Azure DNS over other domain registrars?!  


First of all, it could be difficult to manage many public DNS zones using a graphical interface, and every DNS provider have it's own GUI, which sometimes could be frustrating configuring each of them. This issue leads to automation. If you are working as a DevOps, Azure can help you automate DNS zones configuration by using ARM, PowerShell or Azure CLI. This means that you can manage your DNS records by using the same credentials, APIs, tools and billing as your other Azure services.

With Azure DNS you can create public and private DNS zones. Today, we are going to create a public DNS zone using the Azure CLI, and cover the private zones in another post.

Check the official documentation [What is Azure DNS?](https://docs.microsoft.com/en-us/azure/dns/dns-overview){:target="_blank"}

## Prerequisites
* Azure account
* Azure Storage account

## Create a new public DNS zone
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "public-dns-rg" --location westeurope
```

**Step 3**. Create a new DNS zone:  
```bash
az network dns zone create --name "devcoops.com" --resouce-group "public-dns-rg"
```
Parameters:
* `--name`: name of the public DNS zone.
* `--resource-group`: name of the resource group.
 
Json output will output `nameServers` parameter including the NS records for the zone:  
```json
"nameServers": [
    "ns1-06.azure-dns.com.",
    "ns2-06.azure-dns.net.",
    "ns3-06.azure-dns.org.",
    "ns4-06.azure-dns.info."
  ]
```
**Note**: You can also list the NS records using the command:
```bash
az network dns record-set ns show --name @ --zone "devcoops.com" --resource-group "public-dns-rg"
```

## Create a DNS record
**Step 4**. Now that you have a public DNS zone, create a new DNS record:
```bash
az network dns record-set a add-record --record-set-name "test" --ipv4-address 10.10.10.10 --zone "devcoops.com" --resource-group "public-dns-rg"
```
Parameters:
* `--record-set-name`: name of the DNS A record.
* `--ipv4-address`: value of the DNS A record.

**Step 5**. You can also list the DNS records in the zone:     
```bash
az network dns record-set list --zone "devcoops.com" --resource-group "public-dns-rg"
```

## Test the name resolution
**Step 6**. Open a new *Terminal* tab and run the command:  
```bash
nslookup test.devcoops.com ns1-06.azure-dns.net.
```
Output:  
![Azure CDN create profile result](/assets/images/screenshots/screenshot21.png)

**Note**: You can also use the other 3 NS records listed under **Step 3**.

## Cleaning up

**Step 7**. Delete the DNS A record:  
```bash
az network dns record-set a delete --name "test" --zone "devcoops.com" --resource-group "public-dns-rg"
```

**Step 8**. Delete the public DNS zone:  
```bash
az network dns zone delete --name "devcoops.com" --resource-group "public-dns-rg"
```

**Step 9**. Delete the resource group:  
```bash
az group delete --name "public-dns-rg"
```

## Conclusion
If you ever plan to run your infrastructure on Azure, or if you already have infrastructure running on Azure, it's always a good practice to migrate and host your domains on Azure. 