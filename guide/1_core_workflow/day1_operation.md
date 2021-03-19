#### Understand the terraform core workflow. (write, plan, apply)

## Terraform Core Workflow

* Write
* Plan 
* Apply


#### Setup

> Make sure you are in the correct folder

```bash
# if you are using azure shell
cd ~/clouddrive/tfw/contoso

# Navigate accordingly if you are using your own dev environment
```

---

## Day 1 operation (Create)

**Write**

* Open `main.tf` on the editor and paste in the below code. 

    * If you are in `cloud shell`, you can type **`code .`** and select `main.tf` or simply **`code main.tf`** in terminal to bring up the editor.

```terraform
terraform {
    required_providers {
        azurerm = {
        source  = "hashicorp/azurerm"
        version = "=2.46.0"
        }
    }
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
    features {}
}

resource "azurerm_resource_group" "contoso_rg" {
    name = "contoso_rg"
    location = "UK South"
}
```

* Take a quick look at above code and understand what it does.

    * `~>` is pessismistic constraint operator. Here it tells terraform to use all `azurerm` non-beta versions >=2.13.0 and < 2.14.0

* Save `main.tf` (`ctrl + s` should work on cloud shell)

**Init**

* From terminal, (shortcut `ctrl + '` on cloud shell or vs code)

```bash
# init
terraform init

# Take a look at what's been created    
ls -R ./.terraform/plugins/*/    

# Above command should display the provider that's been installed. 
# Something like "terraform-provider-azurerm_v2.13.0_x5"

```
**Plan**

```bash
# plan. Below command will generate an execution plan.
# Take a few minutes to go through the terminal output and see what changes will be applied

terraform plan
```  

You should see something like below
    
```bash
Plan: 1 to add, 0 to change, 0 to destroy.
```

**Apply**

```bash
# apply
# When prompted to enter a value, type `yes` to approve
terraform apply
```


The terminal output should say something like below

```bash
azurerm_resource_group.contoso_rg: Creating...

azurerm_resource_group.contoso_rg: Creation complete after 1s [id=/subscriptions/.../resourceGroups/contoso_rg]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

**Verify**

Take a look at the Azure portal and verify that the resource has been created.

*  Take a few minutes to understand what `terraform` generated during `apply`

> The terraform `show` command is used to provide human-readable output from a state or plan file. See: https://www.terraform.io/docs/commands/show.html

```bash
# Below should display the current state of your terraform managed infrastructure    
terraform show terraform.tfstate
```
**Important Note**: `.tfstate` will contain sensitive information and must be kept be very secure. You will see that it's in `.gitignore` to make sure it's not accidentally checked into version control.

---

#### Version control your code 

* Add `main.tf` to git

    ```bash
        # Make sure you're in right folder
        cd ~/clouddrive/tfw/contoso
        
        git add .
    ```

* Above should only include `main.tf` because all other items such as `terraform.tfstate`, `.terraform` are in `.gitgnore`. 

* Commit your changes

```bash
git commit -m "created resource group"
```

* _You are welcome to push your changes to your own github remote if you prefer. 

    * For this, You'll have to setup an ssh key using `ssh-keygen` from your cloud shell and add the public key to your github account in order to be able push the repo to your origin. 

    * You'll also have up your `remote` by doing a `git remote add <your_remote_origin_name(e.g: upstream)> <your_remote_url>
    
    * See: https://help.github.com/en/github/using-git/adding-a-remote

    * If you're new to git, and unsure about these steps. Feel free to skip for now, and we can cover these tomorrow when discussing Terraform and DevOps.

---






