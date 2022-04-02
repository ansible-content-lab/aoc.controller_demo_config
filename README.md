[![validation-checks](https://github.com/scottharwell/aoc_demo_setup/actions/workflows/on-push.yml/badge.svg)](https://github.com/scottharwell/aoc_demo_setup/actions/workflows/on-push.yml)

# Ansible on Clouds Demo Configuration

This role is for the Ansible on Clouds team to quickly spin up reusable Ansible Controller configurations. It configures the following components in Ansible Automation Controller:

- Execution Environments
- Credentials
- Projects
- Inventories
- Templates

This role is not published to Ansible Galaxy and therefor must be used directly from scm URLs or through an Execution Environment that has it installed.

## Requirements

This role will work with any Ansible Automation Platform 2.1 version of Ansible Controller, but is intended to be used with Ansible on Clouds deployments.

## Role Variables

### Extra Vars

There are many variables that can be set, with the required variables defined in `defaults/main.yml`.  Below is an example of what is in the file.  Of all the variables set by default, there is one that **must** be set in your execution:

- `rhel_public_key` when using the "Azure - Create RHEL 8 VM" play / template.  Inject an RSA public key string that will be used as the default key by the RHEL host.

```yaml
---
azure_ee_registry_credential_name: Default Execution Environment Registry Credential
azure_demo_templates:
  - name: 'Azure - Create Resource Group'
    playbook: 'create_resource_group.yml'
    extra_vars:
      resource_group_name: "ansible_test"
      region: "eastus"
  - name: 'Azure - Create RHEL 8 VM'
    playbook: 'create_rhel_vm_demo.yml'
    extra_vars:
      resource_group_name: "ansible_test"
      region: "eastus"
      vnet_cidr: "172.16.192.0/24"
      subnet_cidr: "172.16.192.0/25"
      vnet_name: "demo_vnet"
      subnet_name: "demo_subnet"
      network_sec_group_name: "demo_sec_group"
      rhel_public_ip_name: "rhel_demo_ip"
      rhel_nic_name: "rhel_demo_nic"
      rhel_vm_name: "RHEL8-ansible"
      rhel_vm_size: "Standard_DS1_v2"
      rhel_vm_sku: "8_5"
      rhel_admin_user: "azureuser"
      rhel_public_key: ""  # Add your RSA SSH public key in extra vars
      survey_public_ip: "True"
  - name: 'Azure - Create Windows Server 2022 VM'
    playbook: 'create_windows_vm_demo.yml'
    extra_vars:
      resource_group_name: "azure-demo"
      region: "eastus"
      vnet_cidr: "172.16.192.0/24"
      subnet_cidr: "172.16.192.0/25"
      vnet_name: "demo_vnet"
      subnet_name: "demo_subnet"
      network_sec_group_name: "demo_sec_group"
      win_vm_name: "WIN-ansible"
      win_vm_size: "Standard_DS1_v2"
      win_vm_sku: "2022-Datacenter"
      win_public_ip_name: "win_demo_ip"
      win_nic_name: "win_demo_nic"
      win_admin_user: "azureuser"
      win_admin_password: "ansible123!"
  - name: 'Azure - Destroy Resource Group'
    playbook: 'destroy_resource_group.yml'
    extra_vars:
      resource_group_name: "ansible_test"
      region: "eastus"
```

Each of the supported components has an `extra_` key which can be used to supply lists of extra items to configure.  

- `extra_execution_environments`
- `extra_inventories`
- `extra_inventory_sources`
- `extra_credentials`
- `extra_projects`
- `extra_templates`

For instance, if you wanted to add extra credentials, then you would configure the following in your `extra_vars`:

```yaml
---
extra_credentials:
  - name: Local Container Registry
    description: Local Container Registry
    credential_type: Container Registry
    inputs:
      username: scott
      password: "{{ lookup('env', 'REGISTRY_PASS') }}"
```

### Environment Vars

Set the environment variables on your local machine that are required for variables that will be added to Ansible Controller:

| Variable Name         | Description                                                                  |
| --------------------- | ---------------------------------------------------------------------------- |
| CONTROLLER_USERNAME   | Used to authenticate against the Ansible Controller API                      |
| CONTROLLER_PASSWORD   | Used to authenticate against the Ansible Controller API                      |
| AZURE_TENANT_ID       | Used to configure the Azure subscription credential in Automation Controller |
| AZURE_SUBSCRIPTION_ID | Used to configure the Azure subscription credential in Automation Controller |
| AZURE_CLIENT_ID       | Used to configure the Azure subscription credential in Automation Controller |
| AZURE_CLIENT_SECRET   | Used to configure the Azure subscription credential in Automation Controller |
| RED_HAT_ACCOUNT       | Used to configure the Azure subscription credential in Automation Controller |
| RED_HAT_PASSWORD      | Used to configure the Azure subscription credential in Automation Controller |

You can set the variables in your shell with a one-liner:

```bash
export CONTROLLER_USERNAME="username" CONTROLLER_PASSWORD="password" AZURE_TENANT_ID="" AZURE_SUBSCRIPTION_ID="" AZURE_CLIENT_ID="" AZURE_CLIENT_SECRET="" RED_HAT_ACCOUNT="" RED_HAT_PASSWORD=""
```

## Dependencies

- `awx.awx`

## Examples

### Testing

#### Requirements 

The molecule testing configuration is not well defined in this role yet.  In order to test locally, ensure that you have the following installed locally:

- Ansible 2.12 or later
- Ansible Lint 5.4.0 or later
- Ansible Navigator 1.1.0 or later

To install, run:

```bash
# Ensure pip is at the latest version
pip3 install pip --upgrade
# Install Ansible
pip3 install ansible-core ansible-lint ansible-navigator
```

Run the test playbook with `ansible_navigator`.  This will run locally on your PC without containers.

```bash
ansible-navigator run tests/test.yml \
-i tests/inventory \
--pae false \
--mode stdout \
--ee false \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT_ID \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_CLIENT_SECRET \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD
```

You may create an `extra_vars` file at `tests/extra_vars` and include that your test run to change configurations from the defaults that are set in `defaults/main.yml`.  If you do that, then the command above will change to the following:

```bash
ansible-navigator run tests/test.yml \
-i tests/inventory \
--pae false \
--mode stdout \
--ee false \
--extra-vars "@tests/extra_vars" \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT_ID \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_CLIENT_SECRET \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD
```

### Playbook Use

As mentioned earlier, this role is not available on Ansible Galaxy, so you will need to have the role available for a playbook either through an Execution Environment or manual installation.  Once available, referencing the role happens just like any other Ansible role.

```yaml
- hosts: localhost
  roles:
      - role: ansible_on_clouds.aoc_demo_setup
```

## License

GPLv3

## Author

This role was originally written by Scott Harwell and Hicham Mourad from the Ansible team at Red Hat.
