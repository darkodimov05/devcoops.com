---
layout: post
title:  "Add Azure Cloud Shell to Visual Studio Code"
categories: [ azure ]
image: assets/images/azure-cloud-shell-vscode.png
tags: [ azure, azurecloudshell, vscode, visualstudiocode ]
---
Visual Studio Code is a code editor developed by Microsoft, which is currently supported on Mac and Linux as well. It can be customized via extensions. Today we are going to integrate it with the [Azure Cloud Shell](https://shell.azure.com).

## Prerequisites
* Visual Studio Code  
* Azure account with Cloud Shell storage account  

## Configuration
**Step 1**. Open Visual Studio Code, click on **Extensions**, and search for ```Azure Account```.  
![VSCode Extension](/assets/images/screenshots/screenshot2.png)

**Step 2**. Once you installed the extension, go to View -> Command Palette and type ```Azure: Open Bash in Cloud Shell```.  
![VSCode Command Palette](/assets/images/screenshots/screenshot3.png)

The terminal window will display the message:  
```ruby
Sign in needed.
```

**Step 3**. Click the **Sign In** button located on the lower right corner. It will open a new window in the default browser that will require login to the Azure Account. Login to the Azure Portal and go back to VSCode.  
![VSCode Terminal Sign in](/assets/images/screenshots/screenshot5.png)

**Step 4**. Once again open the Command Palette from the View tab and open ```Azure: Open Bash in Cloud Shell```. It should display the following:  
![VSCode Terminal AZ Cloud Shell](/assets/images/screenshots/screenshot6.png)


The official repository: [https://github.com/Microsoft/vscode-azure-account](https://github.com/Microsoft/vscode-azure-account)