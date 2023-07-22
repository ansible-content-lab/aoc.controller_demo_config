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
| `AWS Account`                   | Credential used by the AWS collection for Ansible to communicate with AWS.                                 |
| `Azure Service Principal`       | Credential used by the Azure collection for Ansible to communicate with Microsoft Azure.                   |
| `Azure VM SSH Credential`       | An SSH credential used to connect to and configure VMs on Azure.                                           |
| `Ansible Automation Controller` | A credential to communicate with an instance of Ansible Automation Platform that this role will configure. |

### Execution Environments

| Name                                   | Description                                                                                            |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `Cloud Services Execution Environment` | Latest version of the `ee-cloud-services-rhel8` execution environment published with Ansible on Azure. |
| `Cloud EE`                             | Custom EE with cloud content collections installed                                                     |

### Inventories

| Name        | Description                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `localhost` | An inventory with `localhost` that can be used for collections that do not need to connect to remote hosts (i.e. cloud vendor collections). |
| `Azure`     | A dynamic inventory sourced from the `Azure Service Principal` credential.                                                                  |

### Job Templates

Job templates are configured to pull from Ansible Cloud Content Lab and Validated Content projects:

- [Ansible Cloud Content Lab - Azure Demo](https://github.com/ansible-cloud/azure)
- [Ansible Cloud Content Lab - Azure Roles](https://github.com/ansible-content-lab/azure.infrastructure_config_demos)
- [Ansible Cloud Content Lab - AWS Roles](https://github.com/ansible-content-lab/aws.infrastructure_config_demos)
- [Ansible Validated Content - Azure Demos](https://github.com/redhat-cop/cloud.azure_ops)

### Projects

| Name                                      | Description                                                                       |
| ----------------------------------------- | --------------------------------------------------------------------------------- |
| `Ansible - Azure Demo`                    | A project with playbooks used to demonstrate basic Azure operations.              |
| `Ansible Cloud Content Lab - Azure Roles` | A project that configures the examples in the Azure Cloud Content Lab collection. |
| `Ansible Cloud Content Lab - AWS Roles`   | A project that configures the examples in the AWS Cloud Content Lab collection.   |
| `Azure - CoP Validated Content`           | Validated content from the Ansible CoP team that provides Azure example content.  |

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

There are many variables that can be set, with the required variables defined in `defaults/main.yml`.

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
| `AWS_ACCESS_KEY_ID`     | Used to configure an AWS account credential type for AWS lab content.          |
| `AWS_SECRET_ACCESS_KEY` | Used to configure an AWS account credential type for AWS lab content.          |
| `AWS_SESSION_TOKEN`     | Used to configure an AWS account credential type for AWS lab content.          |
| `RED_HAT_ACCOUNT`       | Used to configure the Red Hat subscription credential in Automation Controller |
| `RED_HAT_PASSWORD`      | Used to configure the Red Hat subscription credential in Automation Controller |

## Dependencies

- `awx.awx`
- `ansible.controller`
