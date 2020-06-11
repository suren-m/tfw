
#### Workspaces

> Take a look at workspaces and how it can help you create separate state files per environment

* https://www.terraform.io/docs/state/workspaces.html

Handy workspace related commands

```bash
terraform workspace list
terrraform workspace new <workspace_name>
terraform workspace select <workspace_name>
```

> Explore .terraform folder and terraform.tfstate.d folders

---

#### Provisioners

> Provisioners help performing tasks such as `remote_exec` after creating a resource.

_for example, installing a few packages after creating a VM_

There are also other provisioners. Have a look at below:

> https://www.terraform.io/docs/provisioners/index.html

---

#### Explore more sub-commands

> Explore more sub commands such as `fmt`, `taint`, `validate` and how they can help.

See : https://learn.hashicorp.com/terraform/certification/terraform-associate-study-guide#learn-more-subcommands

#### Recommended Reading

* Take a few minutes to glance through below docs as well.
    * https://docs.microsoft.com/en-us/azure/developer/terraform/store-state-in-azure-storage
    * https://www.terraform.io/docs/backends/config.html

#### Config Samples from Azure and Terraform

* https://github.com/terraform-providers/terraform-provider-azurerm/tree/master/examples

#### Useful Links 

Below links could help you further in your terraform journey.

* https://learn.hashicorp.com/terraform/azure/intro_az
* https://learn.hashicorp.com/terraform/modules/modules-overview
* https://learn.hashicorp.com/terraform#operations-and-development

* https://www.terraform.io/docs/providers/azurerm/index.html

---

#### Terraform Associate Certification Path

https://learn.hashicorp.com/terraform/certification/terraform-associate-study-guide

---

#### Terraform Whitepapers

https://www.hashicorp.com/resources/unlocking-the-cloud-operating-model-provisioning/?utm_source=terraformselfserviceCTA
