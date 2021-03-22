# Terraform Workshop Guide

### Environment Setup

Make sure you have setup your Azure Subscription and Cloud Shell by following the instructions below.

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
        git clone https://github.com/suren-m/tfw.git
        
        # cd into the repo
        cd tfw

        # We will be using the fictional company `contoso` for labs
        # cd into the contoso folder
        cd contoso

        # Make sure terraform is available in your lab environment
        # This should return something like `v0.12.x`
        terraform version

        # All set for now, get started with the labs when instructed
    ```

* Keep the `guide` folder open in a separate tab / window for convenience. **Please use the version on github** as the source of truth.

   <a target="_blank" href="https://github.com/suren-m/tfw/tree/master/guide">Keep this link open in a new tab or a window</a>

---

Cloud Shell already comes with a Monaco edtior as well as tools such as Terraform, Az CLI and git installed and is highly recommended  for this lab. 
