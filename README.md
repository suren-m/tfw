# Terraform Workshop Guide

### Environment Setup

Make sure you have setup your Azure Subscription and Cloud Shell by following the instructions below.

####  Azure Account Setup from Subscription Pass

> See: [azure_subscription_setup](https://github.com/suren-m/remote-workshop-env/blob/master/azure_subscription/az_subscription_setup.md)

####  Azure Cloud Shell Setup

> See: [cloud_shell_setup](https://github.com/suren-m/remote-workshop-env/blob/master/cloud_shell/cloud_shell_setup.md)


#### Getting started:

* Clone this repo into your `clouddrive` folder in cloud shell.

    *   `clouddrive` folder is backed by fileshare in the storage account created for your cloud shell
    
    <br />

    ```bash
        # make sure you are in the clouddrive folder 
        cd ~/clouddrive

        # clone
        git clone ghttps://github.com/suren-m/tfw.git

        # cd into the labs folder
        cd labs    

        # All set for now, get started with the labs when instructed
    ```

* Keep the `guide` folder open in a separate tab / window for convenience. Please use the version on github as the source of truth.

---

[Optional]

Cloud Shell already comes with a simple edtior as well as tools such as Terraform, Az CLI and git installed and is highly recommended  for this lab. 

However, if you prefer to use cloud-hosted VS Code instead of Azure Cloud shell, then follow instructions below for setting up codespaces.

    > If you're unsure, just go with cloud shell for now.

#### Codespace Environment Setup (VS Code Online)

> See: [codespaces_setup](https://github.com/suren-m/remote-workshop-env/blob/master/codespaces/codespaces_setup.md)

---
