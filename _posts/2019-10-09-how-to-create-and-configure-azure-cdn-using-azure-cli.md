---
layout: post
title:  "How to create and configure Azure CDN using Azure CLI"
categories: [ azure ]
image: assets/images/azure-cdn.jpg
tags: [ azure, azure-cli ]
---
A Content Delivery Network (CDN) is a globally distributed group of nodes / servers that work together in order to deliver content to end users. The idea behind CDN is to provide high performance and high availability, usually by distributing servers across regions that are geographically closer to the users, to minimize latency. These groups of servers are known as points of presence (POPs), and the servers that host the files are called origin servers.  
Most common scenarios include using CDNs for storing cached web static content for blog sites, and media live streaming as well.

## How it works
CDN serves content which stays in a cache defined with a time-to-live (TTL) parameter. User makes request by using URL which is basically an endpoint. The DNS will route the request to the best performing POP location, often the geographically closest edge server to the user. If the file is stored in the cache and the TTL is not expired, the edge server will return the file. But, if the file's TTL parameter is expired, or the file has never been requested, the edge server will send a request to the origin server. The origin server will send the file back to the edge location. Now, the edge server will store the file into the cache and return it to the client.

One of the most popular CDNs including the public cloud providers are: Cloudflare CDN, Azure CDN, AWS CloudFront, Google Cloud CDN.

Check the official documentation [What is a content delivery network on Azure?](https://docs.microsoft.com/en-in/azure/cdn/cdn-overview){:target="_blank"}.

## Prerequisites
* Azure account
* Azure Storage account

{% include in-article-ad.html %}

## Create a new CDN profile
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "cdn-rg" --location westeurope
```

**Step 3**. Create a new CDN profile:  
```bash
az cdn profile create --name "cdnprofile" --resource-group "cdn-rg" --location westeurope --sku Standard_Microsoft
```
Parameters:
* `--name`: name of the CDN profile.
* `--resource-group`: name of the resource group.
* `--location`: location of the CDN profile.
* `--sku`: the pricing tier.
 
Output:  
![Azure CDN create profile result](/assets/images/screenshots/screenshot20.png)

**Step 4**. You can list the CDN profiles:  
```bash
az cdn profile list --resource-group "cdn-rg"
```
It should output the same result from **Step 3**, if you don't have any other CDN profiles.

**Step 5**. You can also get a CDN profile with a specified profile name: 
```bash
az cdn profile show --name "cdnprofile" --resource-group "cdn-rg"
```
It should output the same result from **Step 3**, if you don't have any other CDN profiles.

## Create a new CDN endpoint
**Step 6**. Now that you have a CDN profile, create a CDN endpoint:  
```bash
az cdn endpoint create --name "cdnendpoint1" --resource-group "cdn-rg" --profile-name "cdnprofile" --origin www.example.com
```
If you want to create a CDN endpoint with only HTTPS, just pass the `--no-http` parameter.

**Step 7**. You can also list or get a CDN endpoint with the following commands:     
```bash
az cdn endpoint list --profile-name "cdnprofile" --resource-group "managed-disks-rg"
az cdn endpoint show --name "cdnendpoint1" --resource-group "cdn-rg" --profile-name "cdnprofile"
```
It should output the same result from **Step 5**, if you don't have any other CDN endpoints.

## Cleaning up

**Step 8**. Delete the CDN endpoint:  
```bash
az cdn endpoint delete --name "cdnendpoint1" --resource-group "cdn-rg" --profile-name "cdnprofile"
```

**Step 9**. Delete the CDN profile:  
```bash
az cdn profile delete --name "cdnprofile" --resouce-group "cdn-rg"
```

**Step 10**. Delete the resource group:  
```bash
az group delete --name "cdn-rg"
```

## Conclusion
There are some benefits of using a CDN for your website, which includes:
* Better performance
* Scalable during heavy traffic
* Minimize risk of traffic spikes
* Save on infrastructure cost
* Cached dynamic content
* Act as a reverse proxy