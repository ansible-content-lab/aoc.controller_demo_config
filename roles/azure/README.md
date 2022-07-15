# Azure Setup

This role configures the following components in Ansible Automation Controller that are commonly used for Ansible on Azure demos:

- Execution Environments
- Credentials
- Projects
- Inventories
- Templates
- Workflows

## Requirements

This role will work with any Ansible Automation Platform 2.1+ version of Ansible Controller, but is intended to be used with Ansible on Clouds deployments.

## Role Variables

### Extra Vars

There are many variables that can be set, with the required variables defined in `defaults/main.yml`.  Below is an example of what is in the file.  Of all the variables set by default, there is one that **must** be set in your extra vars:

| Key              | Description                                                                          |
|------------------|--------------------------------------------------------------------------------------|
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

| Variable Name           | Description                                                                  |
|-------------------------|------------------------------------------------------------------------------|
| `CONTROLLER_HOST`       | The hostname of AAP controller to automate against                           |
| `CONTROLLER_USERNAME`   | Used to authenticate against the Ansible Controller API                      |
| `CONTROLLER_PASSWORD`   | Used to authenticate against the Ansible Controller API                      |
| `AZURE_TENANT_ID`       | Used to configure the Azure subscription credential in Automation Controller |
| `AZURE_SUBSCRIPTION_ID` | Used to configure the Azure subscription credential in Automation Controller |
| `AZURE_CLIENT_ID`       | Used to configure the Azure subscription credential in Automation Controller |
| `AZURE_CLIENT_SECRET`   | Used to configure the Azure subscription credential in Automation Controller |
| `RED_HAT_ACCOUNT`       | Used to configure the Azure subscription credential in Automation Controller |
| `RED_HAT_PASSWORD`      | Used to configure the Azure subscription credential in Automation Controller |

You can set the variables in your shell with a one-liner:

```bash
export CONTROLLER_HOST="" CONTROLLER_USERNAME="username" CONTROLLER_PASSWORD="password" AZURE_TENANT_ID="" AZURE_SUBSCRIPTION_ID="" AZURE_CLIENT_ID="" AZURE_CLIENT_SECRET="" RED_HAT_ACCOUNT="" RED_HAT_PASSWORD=""
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

Run the main playbook with `ansible_navigator` using an execution environment with the Azure collection installed.

```bash
ansible-navigator run playbooks/configure_aap_azure.yml \
--pae false \
--mode stdout \
--eei quay.io/scottharwell/cloud-ee:latest \
--eev $HOME/.ssh:/home/runner/.ssh \
--penv CONTROLLER_HOST \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT_ID \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_CLIENT_SECRET \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD \
--senv "SSH_PRIV_KEY={{ lookup('file','~/.ssh/id_rsa_azure_demo') }}" \
--extra-vars "ssh_public_key={{ lookup('file','~/.ssh/id_rsa_azure_demo.pub') }}"
```
