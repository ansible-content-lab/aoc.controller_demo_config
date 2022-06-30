[![validation-checks](https://github.com/scottharwell/aoc_demo_setup/actions/workflows/on-push.yml/badge.svg)](https://github.com/scottharwell/aoc_demo_setup/actions/workflows/on-push.yml)

# Ansible on Clouds Demo Configuration

This collections is for the Ansible on Clouds team to quickly spin up reusable Ansible Controller configurations. It configures the following components in Ansible Automation Controller:

- Execution Environments
- Credentials
- Projects
- Inventories
- Templates

This role is not published to Ansible Galaxy and therefor must be used directly from scm URLs or through an Execution Environment that has it installed.

## Requirements

This role will work with any Ansible Automation Platform 2.1+ version of Ansible Controller, but is intended to be used with Ansible on Clouds deployments.

## Role Variables

### Extra Vars

There are many variables that can be set, with the required variables defined in `defaults/main.yml`.  Below is an example of what is in the file.  Of all the variables set by default, there is one that **must** be set in your extra vars:

| Key               | Description                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------|
| `ssh_public_key`  | The public key used to connect to and manage Linux VMs created in certain playbooks.                                         |


```yaml
---
# Required
ssh_public_key: "ssh-rsa AAAAB3NzaC1yc2EA..."

# Optional
awx_organization: Default
azure_region: eastus
azure_resource_group: ansible_test
windows_admin_password: ansible12345!
```

### Environment Vars

Set the environment variables on your local machine that are required for credentials that will be added to Ansible Controller:

| Variable Name         | Description                                                                  |
|-----------------------|------------------------------------------------------------------------------|
| CONTROLLER_HOST       | The hostname of AAP controller to automate against                           |
| CONTROLLER_USERNAME   | Used to authenticate against the Ansible Controller API                      |
| CONTROLLER_PASSWORD   | Used to authenticate against the Ansible Controller API                      |
| AZURE_TENANT_ID       | Used to configure the Azure subscription credential in Automation Controller |
| AZURE_SUBSCRIPTION_ID | Used to configure the Azure subscription credential in Automation Controller |
| AZURE_CLIENT_ID       | Used to configure the Azure subscription credential in Automation Controller |
| AZURE_CLIENT_SECRET   | Used to configure the Azure subscription credential in Automation Controller |
| RED_HAT_ACCOUNT       | Used to configure the Azure subscription credential in Automation Controller |
| RED_HAT_PASSWORD      | Used to configure the Azure subscription credential in Automation Controller |
| SSH_PRIV_KEY          | Private key used for credential to connect to Linux VMs.                     |

You can set the variables in your shell with a one-liner:

```bash
export CONTROLLER_HOST="" CONTROLLER_USERNAME="username" CONTROLLER_PASSWORD="password" AZURE_TENANT_ID="" AZURE_SUBSCRIPTION_ID="" AZURE_CLIENT_ID="" AZURE_CLIENT_SECRET="" RED_HAT_ACCOUNT="" RED_HAT_PASSWORD="" SSH_PRIV_KEY=""
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

Run the main playbook with `ansible_navigator`.  This will run locally on your PC without containers.

```bash
ansible-navigator run playbooks/main.yml \
--pae false \
--mode stdout \
--ee false \
--penv CONTROLLER_HOST \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT_ID \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_CLIENT_SECRET \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD \
--penv SSH_PRIV_KEY
```

You may create an `extra_vars` file include that your test run to change configurations from the defaults that are set in a role's `defaults/main.yml` file.  If you do that, then the command above will change to the following:

```bash
ansible-navigator run playbooks/main.yml \
--pae false \
--mode stdout \
--ee false \
--extra-vars "@playbooks/env/extra_vars" \
--penv CONTROLLER_HOST \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT_ID \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_CLIENT_SECRET \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD \
--penv SSH_PRIV_KEY \
--eev $HOME/.ssh:/home/runner/.ssh # for ssh key lookups
```

## Installation and Usage

### Installing the Collection with the Ansible Galaxy CLI

Before using the this collection, you need to install it with the Ansible Galaxy CLI:

`ansible-galaxy collection install git+https://github.com/scottharwell/aoc_demo_setup.git`

You can also include it in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: https://github.com/scottharwell/aoc_demo_setup.git
    type: git
    version: main
```

### Playbook Use

As mentioned earlier, this collection is not available on Ansible Galaxy, so you will need to have it available for a playbook either through an Execution Environment or manual installation.  Once available, referencing the role happens just like any other Ansible role.

```yaml
- hosts: localhost
  roles:
    - role: ansible_on_clouds.setup_and_config.aoc_demo_setup
```

## License

GPLv3

## Author

This role was originally written by Scott Harwell and Hicham Mourad from the Ansible team at Red Hat.
