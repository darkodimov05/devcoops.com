---
layout: post
title:  "How to install Terraform CLI on Ubuntu 20.04"
categories: [ terraform ]
image: assets/images/terraform-ubuntu.jpg
tags: [terraform, cloud, ubuntu, linux]
---
Terraform is an open-source tool, created by HashiCorp. It's an Infrastructure as Code tool that enables developers and DevOps to use configuration language called HCL (HashiCorp Configuration Language). Terraform uses a simple syntax and you can provision infrastructure across all the cloud providers and on-premises data centers, which makes it one of the most popular `infrastructure as code` tools. If you need to deploy a `hybrid cloud` or `multicloud` environment, you should definitely want to get to know Terraform.

## Install Terraform on Ubuntu 20.14
**Step 1**. Make sure that your system is up to date and you have installed the following packages
```bash
sudo apt-get update && sudo apt-get install gnupg software-properties-common curl
```
**Step 2**. Add the HashiCorp GPG key
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
```
**Step 3**. Add the HashiCorp Linux repository
```bash
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
```
**Step 4**. Now update the packages to add the repo and install terraform
```bash
sudo apt-get update && sudo apt-get install terraform
```
**Step 5**. Verify the installation
```bash
terraform --version
```
`Output:`
```bash
Terraform v1.0.1
on linux_amd64
```

## Conclusion
This tutorial shows you how to install Terraform CLI on Ubuntu 20.04. For more info please visit the official documentation at [ terraform documentation ](https://www.terraform.io/docs/index.html)