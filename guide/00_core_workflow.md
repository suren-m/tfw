#### Understand the terraform core workflow. (write, plan, apply)

### Terraform Core Workflow

* Write
* Plan 
* Apply

> Make sure you are in the correct folder

```bash
# if you are using azure shell

cd ~/clouddrive/tfw/contoso

# Navigate accordingly if you are using vs code online or local environment
```

#### Day 1 opearation (Create)

* **Write**

* Open main.tf on the editor and paste in the below code. 

    * If you are in `cloud shell`, you can type **`code main.tf`** in terminal to bring up the editor.

```terraform
provider "azurerm" {
    version = ">=2.0.0"
    features {}    
}

resource "azurerm_resource_group" "contoso_rg" {
    name = "contoso_rg"
    location = "UK South"
}
```



* **Init**

    ```bash
    # init
    terraform init

    # Take a look at what's been created    
    ls -R ./.terraform/plugins/*/    

    # Above command should display the provider that's been installed. 
    # Something like "terraform-provider-azurerm_v2.13.0_x5"
    ```
* **Plan**

    ```bash
    # plan. Below command will generate an execution plan.
    # Take a few minutes to go through the terminal output and see what changes will be applied

    terraform plan

    ```  
    * You should see something like below
    
    ```
    Plan: 1 to add, 0 to change, 0 to destroy.
    ```

* **Apply**

    ```bash
    # apply
    # When prompted to enter a value, type `yes` to approve
    terraform apply
   ```

   * The terminal output should say something like below

   ```
    azurerm_resource_group.contoso_rg: Creating...

    azurerm_resource_group.contoso_rg: Creation complete after 1s [id=/subscriptions/.../resourceGroups/contoso_rg]

    Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

   ```

*  Take a look at the Azure portal and verify that the resource has been created.

*  Take a few minutes to understand what `terraform` generated during `apply`

    ```bash
    # Below should display the current state of your terraform managed infrastructure    
    cat terraform.state
    ```
Note: `.tfstate` will contain sensitive information and must be kept be very secure. You will see that it's in `.gitignore` to make sure it's not accidentally checked into version control.

---

### Version control your code 

* Add `main.tf` to git

    ```bash
        # Make sure you're in right folder
        cd ~/clouddrive/tfw/contoso
        
        git add .
    ```

* Above should only include `main.tf` because all others 

You are welcome to p







