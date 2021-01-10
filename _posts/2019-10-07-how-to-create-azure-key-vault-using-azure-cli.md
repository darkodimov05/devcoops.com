---
layout: post
title:  "How to create Azure Key Vault using Azure CLI"
categories: [ azure ]
image: assets/images/azure-key-vault-cli.jpg
tags: [ azure, azure-cli ]
---
**Azure Key Vault** is centralized Azure cloud service that securely stores secrets. A secret can be a password, API, or even certificates. Besides Secrets Management, Azure Key Vault can be used as a Key Management solution which simplifies creation and control of encryption keys used to encrypt data. It can also be used as Certificate Management for provisioning, managing and deploying SSL/TLS certificates, or even provides storing secrets protected by Hardware Security Modules. There are many reasons to use key management solutions, especially working as a DevOps engineer could help you protect secrets that are used in an automation scenario, for example CI/CD pipelines.  
There are two important terms:  
* **Vault owner**: A vault owner have full access and control over the key vault.
* **Vault consumer**: A vault consumer can perform actions inside the key vault based on the given permissions granted by the vault owner.

Official documentation [Azure Key Vault documentation](https://docs.microsoft.com/en-us/azure/key-vault/){:target="_blank"}.

## Prerequisites
* Azure account

{% include in-article-ad.html %}

## Create a Key Vault
**Step 1**. Open *Terminal* and login to the Azure Portal:  
```bash
az login
```
It will open a new window using the default browser where you will be prompted for email and password.

**Step 2**. Create a resource group:  
```bash
az group create --name "key-vault-rg" --location westeurope
```

**Step 3**. Create a Key Vault:  
```bash
az keyvault create --name "devcoopskeyvault" --resource-group "key-vault-rg" --location westeurope
```
Parameters:
* `--name`: name of the Key Vault. It must be a unique name.
* `--resource-group`: name of the resource group where Key Vault will be created.
* `--location`: location of the Key Vault.

The output will be json format which will return a lot of data. The most important properties are:
* **Vault Name**: The name of the vault "devcoopsvault".
* **Vault URI**: the URI that your applications will use through vault API, or in my case: "https://devcoopsvault.vault.azure.net/".

**Note**: The only authorized account that can have access to the key vault is your Azure account, unless you configure Access Control and Access Policies.

## Add a secret to the Key Vault

**Step 4**. Let's store a secret by running the following command:  
```bash
az keyvault secret set --vault-name "devcoopskeyvault" --name "DBPassword" --value "NX5HeUzm34t2cGXA"
```
Parameters:
* `--vault-name`: name of the Key Vault where we want to store a secret.
* `--name`: name of the secret. In my case i want to store database password, so i named it "DBPassword".
* `--value`: Password for the database.

**Note**: The password is random generated on [Strong Random Password Generator](https://passwordsgenerator.net/){:target="_blank"}. My two cents on password best practices is using passphrase instead of passwords.

**Step 5**. To view the password value as a plain text, use the following command:  
```bash
az keyvault secret show --name "DBPassword" --vault-name "devcoopskeyvault"
```
The password will be shown in the **value** parameter.

## Cleaning up

**Step 7**. Remove key vault secret:  
```bash
az keyvault secret delete --name "DBPassword" --vault-name "devcoopskeyvault"
```

**Step 8**. Delete the key vault:  
```bash
az keyvault delete --name "devcoopskeyvault" --resource-group "key-vault-rg"
```

**Step 9**. Delete the resource group:  
```bash
az group delete --name "key-vault-rg"
```

## Conclusion
Storing plain text passwords and unprotected certificate keys often leads to a security breach, which can easily be prevented using Azure Key Vault.