---
layout: post
title:  "How to buy custom domain name with Azure App Service Domains"
categories: [ azure ]
image: assets/images/azure-app-service-domain.jpg
tags: [ azure ]
---
Azure App Service Domains let you buy and manage top-level domains directly from the Azure Portal. These domains will be hosted on the Azure DNS Service, that you can easily map to your existing Web Apps or Virtual Machines. Azure DNS provides certain advantages like performance and security within the Microsoft Azure ecosystem. I have already talked about creating custom public DNS zones in the last [post]({% post_url 2019-10-10-how-to-create-and-manage-public-azure-dns-using-azure-cli %}){:target="_blank"}, so if you got the time please read it.  

There are currently 9 top-level domains that are supported by Azure App Service Domains:
* .com
* .net
* .co.uk
* .org
* .nl
* .in
* .biz
* .org.uk
* .co.in

Check the official documentation [Buy a custom domain name for Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/manage-custom-dns-buy-domain){:target="_blank"}.

## Prerequisites
* Azure account
* Remove the spending limit on your subscription

{% include in-article-ad.html %}

## Create App Service domain
**Step 1**. Login to the Azure Portal.

**Step 2**. Go to the Azure Marketplace, search for "Azure App Domains, and open the service. It should display a blank page with the following button in the center:  
![Create App Service Domain button](/assets/images/screenshots/screenshot22.png)

**Step 3**. Click **Create App Service domain**. It will open another view:  
![Create App Service Domain main view](/assets/images/screenshots/screenshot23.png)

**Step 4**. Now, enter the domain name you want to purchase. In my example i have used the "devcoops.com" domain to see what will happen.  
![Create App Service Domain main view domain-name](/assets/images/screenshots/screenshot24.png)

**Note**: Since **.com** domain is already in use, as you can see, it automatically mark the first available domain, and in my case taht is "devcoops.biz". Pretty good domain name suggestions.

**Step 5**. In the next field, you'll have to choose type of subscription and resource group. Let's create new resource group named *app-service-domain-rg*.  
![Create App Service Domain main view subscription-and-resource-group](/assets/images/screenshots/screenshot25.png)

**Step 6**. Next, we got the *Contact Information* form, where we need to add the required fields like: First Name, Last Name, Email,  Country Code,  Phone, Address 1, Country, State or teritory, City and finally Postal Code. It's pretty much standard procedure to registar a domain just as you would with any other domain name provider.  
![Create App Service Domain main view contact-information](/assets/images/screenshots/screenshot26.png)

**Step 7**. Once you fill in the *Contact Information* click **OK**. Now, open the *Privacy protection* view.   
![Create App Service Domain main view privacy-protection](/assets/images/screenshots/screenshot27.png)

**Note**: As you can see, by default *privacy protection* is turned on. But, you cannot use on the following domains: *co.uk*, *in*, *org.uk*, *co.in*. *Privacy *Protection* doesn't come with additional cost, so it's a nice benefit to have.

**Step 8**. On the last view *Legal Terms*, there is the pricing details and the legal terms as well.  
[Create App Service Domain main view legal-terms](/assets/images/screenshots/screenshot28.png)

**Note**: As you can see from the screenshot, Azure App Service Domains is using [GoDaddy](godaddy.com){:target="_blank"} for domain registration.

**Step 9**. Click the **Create** button, and you'll have to wait for the validation of the domain order.

## Conclusion
As everything already migrating to the cloud, I think it's worth to purchase and manage custom domains using the Azure App Service Domains, if you are already using other Azure Services, mainly because of two things:
* GoDaddy is one of the most, if not the most popular domain name registrar.
* You can manage almost everything from the Azure Portal, or even could use automation options like ARM templates, PowerShell and Azure CLI. 