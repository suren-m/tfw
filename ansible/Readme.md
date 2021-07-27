#### Below tips are meant to be used just as a checklist for setting up Ansible and running the first playbook on an Azure VM. 

#### Ansible only runs on linux, so this guide also assumes the reader is somewhat familiar with linux based environments.

#### Please note that this is not a lab guide as this is a more advanced section. 
#### Please refer to official Ansible and Msft docs for upto date guides, as the items here are prone to change.

---

#### 1. Setup Ansible

* Provision a linux VM (Ubuntu 20.04 perhaps), preferably in the same VNET as targets, so it can be used to Run Ansible playbooks. 
* Avoid using Public-IPs if you already have access to VNET through VPN / VNET Gateways or Express Routes.

  * If using Terraform, see 
    * https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/virtual_machine
  
  * Or for MSFT docs using CLI based approach, see below (note that this uses CentOS as an example but you can always use other distros such as Ubuntu 20.04 as well) 
    * https://docs.microsoft.com/en-us/azure/developer/ansible/install-on-linux-vm?tabs=azure-cli
  
* If using WSL2, ensure it's only used for dev/lab environments. (see below for more details)
  * https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html 

---

#### 2. Install and Configure Ansible on the Linux environment

* Ansible depends on Python based ecosystem. (ideally just use python3 based tools whenever possible)

* Install python3, pip3, etc. (You may be needing more dependencies such as python3-setuptools later. This can be installed via pip3 as well)
  * Use `apt` if using ubuntu - https://docs.microsoft.com/en-us/azure/developer/ansible/install-on-linux-vm?tabs=azure-cli#install-ansible-on-the-virtual-machine

* Installing Ansible
 *  Two option available. Pip3 or package managers. It's important to know both.
 *  See below: 
     * https://docs.microsoft.com/en-us/azure/developer/ansible/install-on-linux-vm?tabs=azure-cli#ansible-29-with-the-azure_rm-module
     * https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu

---

#### 3. Install `Azure` module to connect to Azure environments

* You can install it using `pip3 install ansible[azure]` on `ansible-2.9` but there are other options for `ansible-2.10+` such as using `ansible collections`
* For official docs, see:
* https://docs.ansible.com/ansible/latest/collections/azure/azcollection/azure_rm_inventory.html
* https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html

---

#### 4. Ensure the ansible environment has access to azure environment

* This can be done by setting `~/.azure/credentials` file or with environment variables. See below:
* https://docs.microsoft.com/en-us/azure/developer/ansible/create-ansible-service-principal?tabs=azure-cli
* For complete guide, see: https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html#authenticating-with-azure

* Try creating a resource-group via ansible-playbook to further ensure ansible has access.
* https://docs.microsoft.com/en-us/azure/developer/ansible/getting-started-cloud-shell?tabs=ansible#create-an-azure-resource-group

---

#### 5. Deploy target VMs (windows or linux)
* Note that if configuring linux VMs, ansible would rely on mechansims such as `ssh` to authenticate.
* If managing Windows VM, then ansible would rely on WinRM.
* Provision the desired targets.

---

#### 6. Query the inventory dynamically.
* Targets can be retrieved dynamically by using below: (adjust the values accordingly)
* Note that the name of the inventory file has to end with `azure_rm.yml`

```
plugin: azure_rm
include_vm_resource_groups:
  - ansible-inventory-test-rg
auth_source: auto
conditional_groups:
  linux: "'CentOS' in image.offer"
  windows: "'WindowsServer' in image.offer"
```

Or if grouping based on more generic level and then filtering out just windows machines based on tags (such as os = windows)

```
plugin: azure_rm
plain_host_names: yes
include_vm_resource_groups:
  - azenv-uks
auth_source: auto
conditional_groups:
  linux: "'linux' in os_profile.system"
  windows: "'windows' in os_profile.system"
exclude_host_filters:
  - tags.os != 'windows' 
```

* Inventories can be retrieved using following commands

```
# Use your filename here. (just ensure the suffix is correct)
ansible-inventory -i windows_azure_rm.yml --graph
ansible-inventory -i windows_azure_rm.yml --list
```
For more, see: https://docs.microsoft.com/en-us/azure/developer/ansible/dynamic-inventory-configure?tabs=azure-cli

---

#### 7. `WinRM` listener dependency for managing windows VMs
* Requires setting `WinRM` listener first

> Dependency - `pip install "pywinrm>=0.3.0"` (feel free to try pip3 as well)

See:
   * https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#what-is-winrm
   * https://docs.microsoft.com/en-us/azure/developer/ansible/vm-configure-windows?tabs=ansible#configure-the-winrm-listener

Example:

```
---
# ansible-inventory -i windows_azure_rm.yml --list | jq '.windows' > targets.json
# ansible-playbook winrm.yml --extra-vars="@targets.json" --extra-vars="rg_name=azenv-uks"
- name: Setup winRM for Azure VMs
  hosts: localhost
  connection: local
  tasks:
    - name: Create VM script extension to enable HTTPS WinRM listener
      azure_rm_virtualmachineextension:
        name: winrm-extension
        resource_group: "{{ rg_name | default('az-env-uks') }}" # pass it via extra-vars
        virtual_machine_name: "{{ item }}"
        publisher: Microsoft.Compute
        virtual_machine_extension_type: CustomScriptExtension
        type_handler_version: "1.9"
        settings: '{"fileUris": ["https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"],"commandToExecute": "powershell -ExecutionPolicy Unrestricted -File ConfigureRemotingForAnsible.ps1"}'
        auto_upgrade_minor_version: true
      with_items: "{{ hosts }}"
    - name: wait for the WinRM port to come online
      wait_for:
        port: 5986
        host: "{{ item }}"
        timeout: 600
      with_items: "{{ hosts }}"
```

---

#### 8. Configure Windows VMs

* Feel free to change hosts to `all` if all the items in inventory are windows, and not grouped by OS

** This is example just uses basic auth. For more advanced auth options, please see below: **
* https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#authentication-options

```
- hosts: windows
  vars:
    ansible_user: "{{ lookup('env', 'winuser') }}"
    ansible_password: "{{ lookup('env', 'winpass') }}"
    ansible_connection: winrm
    ansible_winrm_transport: ntlm
    ansible_winrm_server_cert_validation: ignore
  tasks:
    - name: Test connection
      win_ping:

    - name: Ensure 7-Zip is installed via Chocolatey
      win_chocolatey:
        name: 7zip
        state: present    
```

Also look at features such as `ansible-vault` for encrypting data. 
* https://docs.ansible.com/ansible/latest/user_guide/vault.html

---
