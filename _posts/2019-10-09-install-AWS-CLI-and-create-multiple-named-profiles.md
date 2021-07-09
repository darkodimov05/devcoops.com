---
layout: post
title:  "How to install AWS CLI and setup multiple named profiles on Ubuntu"
categories: [ aws ]
image: assets/images/awscli.jpg
tags: [ aws, cloud, aws services, linux ]
---
The AWS CLI is an Amazon Command Line Interface that communicates with the AWS API and allows you to manage your AWS services from a terminal session from your local machine. The AWS CLI is a powerfull tool because it enables developers and devops engineers to have a full control over their Amazon public cloud services by typing commands on a specified line. With AWS CLI you can do everything that is also possible with the AWS Management Console. For an example you can make efficient file transfers to and from Amazon S3 -> [how to copy from AWS S3 bucket to Azure Blob Storage](https://jenkins.io/doc/). In this tutorial we will guide you through the installing process of AWS CLI and we will see how you can create a multiple named profiles and switch over them.

## Prerequisites
* AWS account
* Make sure that you are logged into your Linux machine as a user with `sudo` privileges

## Installing AWS CLI
**Step 1**. Before we can install the AWS CLI on our machine we have to make sure that python and pip are already installed. To make sure run the following commands:
```bash
sudo python3 -V
sudo pip3 -V
```
If the output shows you that `no packages are installed` you need to follow the next steps.

**Step 2**. Next we will show you how to install Python3 and pip for Python3. You need to complete the following steps to install Python3 along with pip3.

First update the package list using the following command and install the prerequisites:
```bash
sudo apt update
sudo apt install software-properties-common
```
**Step 3**. Now add the deadsnakes PPA to your sources list:
```bash
sudo add-apt-repository ppa:deadsnakes/ppa
```
Press `Enter` to continue and install Python3.7:
```bash
sudo apt install python3.7
```
**Step 4**. To install pip for Python 3 run:
```bash
sudo apt install python3-pip
```
Now we can make sure that pip3 is installed by checking the pip version with the following command:
```bash
pip3 --version
```
The `Output` should look something like this:
```bash
pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.6)
```
Once you are finished with the python and pip installation we can continue with the AWS CLI installation.

**Step 4**. We can install AWS CLI simply by running the following command:
```bash
pip3 install awscli --upgrade --user
```
The `--upgrade` option tells pip3 to upgrade any requirements that are already installed. The `--user` option tells pip3 to install the program to a subdirectory of your user directory to avoid modifying libraries used by your operating system. 

If you followed the above steps you have successfully installed `AWS CLI`. Just to make sure run:
```bash
aws --version
```
The `Output` should be similar to the following:
```bash
aws-cli/1.16.253 Python/3.7 Linux/5.0.0-31-generic botocore/1.12.243
```
Also, if you want to check which packages are `outdated` use the following command:
```bash
sudo pip3 list -o
```
`Output`:
```bash
Package      Version  Latest   Type 
------------ -------- -------- -----
asn1crypto   0.24.0   1.0.1    wheel
awscli       1.16.253 1.16.255 wheel
botocore     1.12.243 1.12.245 wheel
cryptography 2.1.4    2.7      wheel
idna         2.6      2.8      wheel
ipaddress    1.0.17   1.0.22   wheel
keyring      10.6.0   18.0.1   wheel
keyrings.alt 3.0      3.1.1    wheel
pygobject    3.26.1   3.34.0   sdist
pyxdg        0.25     0.26     wheel
rsa          3.4.2    4.0      wheel
setuptools   39.0.1   41.4.0   wheel
six          1.11.0   1.12.0   wheel
wheel        0.30.0   0.33.6   wheel
```
If you find your current version in the table, you can upgrade it with the following command:
```bash
pip3 install --upgrade --user awscli
```
## Create multiple named profiles

**Step 1**. If you navigate to your home directory and list all the files you should see a hidden `.aws` folder:
```bash 
ls -la ~
```
Navigate to the AWS directory:
```bash
cd ~/.aws/
```
**Step 2**. Here you will find two configuration files `config` and `credentials`. In the fist one `config` you need to configure your profiles by simply setting up the `region` and the output format. 
The `config` file should look like the following:
```python
[default]
output = json
region = eu-west-1

[profile user1]
output = json
region = eu-west-1

[profile user2]
output = json
region = eu-west-1
```
The AWS CLI supports three different output formats:
* JSON (json) 
* Tab-delimited text (text) 
* ASCII-formatted table (table) 

In the second one `credentials` are stored the user `aws_access_key_id` and `aws_secret_access_key` which you can find them using the AWS Management Console, open the `IAM` service, select the user that you want to use and navigate to the `Security Credentials` section.

**Step 3**. Once you have the `aws_access_key_id` and `aws_secret_access_key` of the user you should put them into the `credentials` file:
```python
[default]
aws_access_key_id = AKIA44QJSDKDJSKHMHZYZ
aws_secret_access_key = U7uyCrEfVC4JP7LdsadsgsagsapQgawI88BsU2ByrJgyMOajaE

[user1]
aws_access_key_id = AKIAVQUFPSJGLSSLVVPJ
aws_secret_access_key = wgsau4Lq5JbU0-Cgdasdahsadgsagsagsay1wsWBozPZMrprln9

[user2]
aws_access_key_id = AFSSGASXKCN3LHMHZYZ
aws_secret_access_key = gsauyCrEfVC4JP7LpdhdhdshhdsvQgawI88BsUgsaasfyMOajaE
```

**Step 4**. Finally to switch over the profiles you can use the following command:
```bash
export AWS_PROFILE=user1
```
To switch your second user use the same command:
```bash
export AWS_PROFILE=user2
```
Before making any actions on your AWS profile you need to make sure which profile you are currently using by running:
```bash
echo $AWS_PROFILE
```
`Output`:
```bash
user2
```

## Conclusion
In this tutorial we learned how to install and use AWS CLI, and how to switch over your AWS profiles. For more details on how to use `AWS CLI` you can find at [AWS CLI Documentation](https://aws.amazon.com/cli/).