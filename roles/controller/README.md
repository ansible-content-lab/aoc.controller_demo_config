# Ansible Automation Controller Setup

This role configures the following components in Ansible Automation Controller that are commonly used for Ansible on Clouds demos:

- Credentials
- Execution Environments
- Inventories
- Projects
- Templates
- Workflows

## AAP Components

### Credentials

| Name                            | Description                                                                                                |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `Red Hat Registry`              | A credential to `console.redhat.com` that will allow retrieval of certified execution environments.        |
| `Azure Service Principal`       | Credential used by the Azure collection for Ansible to communicate with Microsoft Azure.                   |
| `Azure VM SSH Credential`       | An SSH credential used to connect to and configure VMs on Azure.                                           |
| `Ansible Automation Controller` | A credential to communicate with an instance of Ansible Automation Platform that this role will configure. |

### Execution Environments

| Name                                   | Description                                                                                            |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `Cloud Services Execution Environment` | Latest version of the `ee-cloud-services-rhel8` execution environment published with Ansible on Azure. |
| `Harwell - Cloud EE`                   | Custom EE with cloud content collections installed                                                     |

### Inventories

| Name        | Description                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `localhost` | An inventory with `localhost` that can be used for collections that do not need to connect to remote hosts (i.e. cloud vendor collections). |
| `Azure`     | A dynamic inventory sourced from the `Azure Service Principal` credential.                                                                  |

### Job Templates

Most of the job templates are configured to pull another Cloud Content Lab project, [Azure Infrastructure Config Demos](https://github.com/ansible-content-lab/azure.infrastructure_config_demos).

| Name                                     | Description                                                                                                                                                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `lab.azure_roles.create_resource_group`  | Simple playbook that creates a resource group.                                                                                                                                                                |
| `lab.azure_roles.delete_resource_group`  | Deletes a resource group and all resources within that group.                                                                                                                                                 |
| `lab.azure_roles.create_rhel_vm`         | Creates a RHEL-based VM and all of the Azure resources required to deploy the VM.                                                                                                                             |
| `lab.azure_roles.delete_rhel_vm`         | Deletes the RHEL-based VM and its associated resources.                                                                                                                                                       |
| `lab.azure_roles.create_windows_vm`      | Creates a Windows Server-based VM and all of the Azure resources required to deploy the VM.                                                                                                                   |
| `lab.azure_roles.delete_windows_vm`      | Deletes the Windows Server-based VM and its associated resources.                                                                                                                                             |
| `lab.azure_roles.create_transit_network` | Creates a hun-and-spoke network model with a hub and two spoke networks.  One spoke acts as a DMZ.                                                                                                            |
| `lab.azure_roles.delete_transit_network` | Deletes the transit network and its related resources.                                                                                                                                                        |
| `lab.azure_roles.update_rhel_vms`        | A simple playbook that demonstrates running `dnf upgrade -y` on RHEL VMs.                                                                                                                                     |
| `Azure Demos - Ephemeral Workload Test`  | A playbook that will create a RHEL VM, run an operation on the VM, and then delete the VM to simulate an ephemeral VM workload.  This has been superseded by the workflow that is also deployed in this role. |

### Projects

| Name                                      | Description                                                                       |
| ----------------------------------------- | --------------------------------------------------------------------------------- |
| `Ansible - Azure Demo`                    | A project with playbooks used to demonstrate basic Azure operations.              |
| `Ansible Cloud Content Lab - Azure Roles` | A project that configures the examples in the Azure Cloud Content Lab collection. |
| `Ansible Cloud Content Lab - AWS Roles`   | A project that configures the examples in the AWS Cloud Content Lab collection.   |

### Workflows

| Name                                                       | Description                                                                                                                                                                                                                                   |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ansible_on_clouds.setup_and_config.ephemeral_vm_workload` | A workflow that uses multiple job templates that create a VM and its dependencies, then runs a command on the VM, and then deletes the VM when done.  This simulates an ephemeral VM workload that may happen nightly in spot instances.      |
| `lab.azure_roles.install_log_analytics_workflow`           | A workflow that installs the Azure Log Analytics agent onto VMs and creates a log analytics workspace to store those logs. This workflow is used to prepare VMs for the following role in most cases since the Azure Log Agent is deprecated. |
| `lab.azure_roles.arc_log_migration`                        | A workflow that converts VMs from using the Azure Log Agent to Azure Monitoring Agent via Azure Arc.                                                                                                                                          |

## Requirements

This role will work with any Ansible Automation Platform 2.1+ version of Ansible Controller, but is intended to be used with Ansible on Clouds deployments.

## Role Variables

### Extra Vars

There are many variables that can be set, with the required variables defined in `defaults/main.yml`.  Below is an example of what is in the file.  Of all the variables set by default, there is one that **must** be set in your extra vars:

| Key              | Description                                                                          |
| ---------------- | ------------------------------------------------------------------------------------ |
| `ssh_public_key` | The public key used to connect to and manage Linux VMs created in certain playbooks. |

```yaml
---
# Required
ssh_public_key: "{{ lookup('file', '~/.ssh/id_rsa_azure_demo.pub') }}"

# Optional
awx_organization: Default
azure_region: eastus
azure_resource_group: ansible_test
windows_admin_password: ansible12345!
```

### Environment Vars

Set the environment variables on your local machine that are required for credentials that will be added to Ansible Controller:

| Variable Name           | Description                                                                    |
| ----------------------- | ------------------------------------------------------------------------------ |
| `CONTROLLER_HOST`       | The hostname of AAP controller to automate against                             |
| `CONTROLLER_USERNAME`   | Used to authenticate against the Ansible Controller API                        |
| `CONTROLLER_PASSWORD`   | Used to authenticate against the Ansible Controller API                        |
| `AZURE_TENANT_ID`       | Used to configure the Azure subscription credential in Automation Controller   |
| `AZURE_SUBSCRIPTION_ID` | Used to configure the Azure subscription credential in Automation Controller   |
| `AZURE_CLIENT_ID`       | Used to configure the Azure subscription credential in Automation Controller   |
| `AZURE_CLIENT_SECRET`   | Used to configure the Azure subscription credential in Automation Controller   |
| `RED_HAT_ACCOUNT`       | Used to configure the Red Hat subscription credential in Automation Controller |
| `RED_HAT_PASSWORD`      | Used to configure the Red Hat subscription credential in Automation Controller |

You can set the variables in your shell with a one-liner:

```bash
export CONTROLLER_HOST="" CONTROLLER_USERNAME="username" CONTROLLER_PASSWORD="password" AZURE_TENANT_ID="" AZURE_SUBSCRIPTION_ID="" AZURE_CLIENT_ID="" AZURE_CLIENT_SECRET="" RED_HAT_ACCOUNT="" RED_HAT_PASSWORD=""
```

## Dependencies

- `awx.awx`
- `ansible.controller`

## Examples

### Testing

#### Requirements

The molecule testing configuration is not well defined in this role yet.  In order to test locally, ensure that you have the following installed locally:

- Ansible 2.12 or later
- Ansible Lint 6.2.0 or later
- Ansible Navigator 1.1.0 or later

To install, run:

```bash
# Ensure pip is at the latest version
pip3 install pip --upgrade
# Install Ansible
pip3 install ansible-core ansible-lint ansible-navigator
```
