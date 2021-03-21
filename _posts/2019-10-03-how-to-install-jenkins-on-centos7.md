---
layout: post
title:  "How to install Jenkins on Centos 7"
categories: [ Centos 7 ]
image: assets/images/jenk.jpg
tags: [ jenkins, cloud, pipelines, centos7, continuous integration ]
---
Jenkins is an open-source tool a continuous integration and continuous delivery tool written in Java. Basically it will build, test and deploy your software projects.

Jenkins is primarily a build and release tool that was written by the original community as a build and release tool. They did not target it as a continuous integration continuous deployment or an orchestration tool, it was simply used for releasing builds to production. But now Jenkins has a lot of other uses like, it's a continuous integration tool that allows developers to make sure that their environments have the exact same code as their code repositories. Let's say there is a dev environment. If Jenkins has uses CI tool in an environment and makes sure that every comment that goes to a repository is mirrored in the environment as well. Jenkins is also used as a continuous deployment tool. Every time you do a commit it allows you to push your code to production through a series of quality gates and a lot of tests. it is also used as an orchestration tool, so people use it as a scheduler or an Orchestrator to kick off their Chef or Ansible playbooks or any other ad-hoc scripts.

In this tutorial, we will walk you through the steps of installing Jenkins on a CentOS 7 using the official Jenkins repository.

## Prerequisites
To continue with this tutorial, make sure that you are logged in as a user with `sudo` privileges.

{% include in-article-ad.html %}

## Installing Jenkins
You can start with the installation on your CentOS system by following the steps:

**Step 1**. As we mentioned above, Jenkins is a java based application, so first, we need to install Java.
```bash
sudo yum install java-1.8.0-openjdk-devel
```
**Step 2**. Next we need to enable the Jenkins repository. First we will import the GPG key:
```bash
curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
```
Now add the repository to your system:
```bash
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
```
**Step 3**. Once it is done, we will install the latest version of Jenkins with `yum` on your CentOS system:
```bash
sudo yum install jenkins
```
Once the installation is done, you can start the Jenkins service with:
```bash
sudo systemctl start jenkins
```
To make sure that Jenkins is started run:
```bash
sudo systemctl status jenkins
```
The `Output` should be similar to this:
```ruby
● jenkins.service - LSB: Jenkins Automation Server
   Loaded: loaded (/etc/rc.d/init.d/jenkins; bad; vendor preset: disabled)
   Active: active (running) since Wed 2019-10-02 20:21:30 EDT; 9s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 2606 ExecStart=/etc/rc.d/init.d/jenkins start (code=exited, status=0/SUCCESS)
    Tasks: 11
   Memory: 163.9M
   CGroup: /system.slice/jenkins.service
           └─2625 /etc/alternatives/java -Dcom.sun.akuma.Daemon=daemonized -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins...
```
Now we are sure that Jenkins has been succesfully installed. To enable your Jenkins service to start on system boot run:
```bash
sudo systemctl enable jenkins
```

## Setting up the Firewall
If your CentOS server is proteced by a firewall you will have to allow port `8080`. To do this use the following commands:
```bash
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --reload
```

## Accessing Jenkins
To access Jenkins, open your browser and type your domain or IP address together with the port `8080`:

![Access Jenkins](/assets/images/screenshots/jenkins_1.jpg)

To print the password use the following command:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
The `Output` should be similar as shown below:
```bash
279fda1015ab4632a8b011ab37ae9cad
```
Copy this alphanumeric password and paste it into the Administrator password field and click on the `Continue` button.
Once you click on the Continue button you will be redirected on the customatization page:

![Customize Jenkins](/assets/images/screenshots/jenkins_2.jpg)

Click on the `Install suggested plugins` to start the installation. Once the installation is done you will have to `Create your first Admin user` and put your desired `Jenkins URL`. 

After your succesfull configuration click on the `Start Using Jenkins` button and you will be redirected to the Jenkins dashboard logged in as the admin user. Now you should be able to create your first Jenkins `job`: 

![First Jenkins job](/assets/images/screenshots/jenkins_3.jpg)

In this tutorial we learned how to install and setup Jenkins on CentOS 7 machine. You can also visit the official [Jenkins Documentation](https://jenkins.io/doc/) on how to start using Jenkins.
