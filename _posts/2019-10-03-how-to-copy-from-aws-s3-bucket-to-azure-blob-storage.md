---
layout: post
title:  "How to copy from AWS S3 bucket to Azure Blob Storage"
categories: [ azure, aws ]
image: assets/images/aws-s3-to-azure-blob-storage.jpg
tags: [aws, azure, azcopy, azure blob]
---
When we mention public cloud and devops, we often think of Amazon Web Services, Microsoft's Azure or Google Cloud Platform. As AWS becomes dominant public cloud leader through the years, many corporations and startups started to migrate their on-premises infrastructure to the cloud, especially AWS. This often leads to public cloud vendor lock-in. But, if someday you decide to migrate to Azure, you might face up many challenges. For example, if you want to migrate your S3 bucket to Azure Storage, you will have to write your own custom scripts or even come up with your own custom tools. This adds to complexity. So, in today's blog, we are going to talk about how to migrate/copy AWS S3 buckets to Azure Blob Storage using a tool called **azcopy** that will help you move your AWS S3 buckets to the Azure storage service.

## What is AzCopy
**AzCopy** is a command-line utility that can copy data to or from an Azure storage accounts. The current version is AzCopy v10.2.1

[AzCopy repository](https://github.com/Azure/azure-storage-azcopy){:target="_blank"}

## Prerequisites
* Azure account
* Azure storage account
* AWS account
* AWS S3 bucket

## Download and Install azcopy
**Step 1**. Download **azcopy** as a zip/tar from Microsoft:
* [Windows](https://aka.ms/downloadazcopy-v10-windows)
* [Linux](https://aka.ms/downloadazcopy-v10-linux)
* [MacOS](https://aka.ms/downloadazcopy-v10-mac)

**Step 2**. Install **azcopy** by extracting the archive file to the home directory or anywhere you like, and add the executable to the *PATH*. In my example, i have moved the zip file to my $HOME directory and extracted it.
For Linux/MacOS using terminal:
```bash
export PATH=$PATH:~/azcopy_darwin_amd64_10.2.1
```

**Note**: You can check the path using *echo $PATH* command.

For Windows using CMD:
```bash
pathman /au c:\azcopy_darwin_amd64_10.2.1
```

**Note**: You can check the path using *set PATH* command. Restart the service if the path is not shown.

**Step 3**. Test the installation, by running the *azcopy* command.

## Authorize AzCopy with Azure and AWS

**Step 4**. First, authorize AzCopy with Azure using the following command:
```bash
azcopy login --tenant-id <active-directory-tenant-id>
```

**Note**: To find the Tenant ID, go to **Azure Active Directory > Properties > Directory ID** in the Azure Portal. Also, don't forget to assign the **Storage Blob Data Contributor** role to the Azure user.

**Step 5**. Open up a web browser, go to the page: [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin){:target="_blank"} and enter the code.  
![Azure device code](/assets/images/screenshots/screenshot11.png)

**Step 6**. Next, login with the Azure account. It will display the following message:  
![Azure Storage AzCopy message](/assets/images/screenshots/screenshot12.png)

**Step 7**. Configure AWS by setting *AWS_ACCESS_KEY_ID* and *AWS_SECRET_ACCESS_KEY* environment variables.
```bash
export AWS_ACCESS_KEY_ID=<insert-aws-access-key-id>
export AWS_SECRET_ACCESS_KEY=<insert-aws-secret-access-key-here>
```

Once the authorization is configured, you can try to run the following **azcopy** commands:  
**Copy an AWS S3 object to Azure blob**
```bash
azcopy cp "https://s3.amazonaws.com/devcoopsbucket/index.html" "https://devcoopsstorage.blob.core.windows.net/devcoopscontainer/"
```

**Copy an AWS S3 folder to Azure Storage**
```bash
azcopy cp "https://s3.amazonaws.com/devcoopsbucket/folder1" "https://devcoopsstorage.blob.core.windows.net/devcoopscontainer/folder1 --recursive=true"
```

**Copy an AWS S3 bucket to Azure blob storage**
```bash
azcopy cp "https://s3.amazonaws.com/devcoopsbucket" "https://devcoopsstorage.blob.core.windows.net/devcoopscontainer --recursive=true"
```

**Copy all AWS S3 buckets in all regions to Azure Storage**
```bash
azcopy cp "https://s3.amazonaws.com/" "https://devcoopsstorage.blob.core.windows.net --recursive=true"
```

**Copy all AWS S3 buckets in a specific region to Azure Storage**
```bash
azcopy cp "https://s3.eu-west-1.amazonaws.com/" "https://devcoopsstorage.blob.core.windows.net --recursive=true"
```

## Conclusion
**AzCopy** is a fast and simple tool, which can also be scheduled as part of backup and/or sync operations.

Official documentation: [Copy data from Amazon S3 buckets by using AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-s3){:target="_blank"}.