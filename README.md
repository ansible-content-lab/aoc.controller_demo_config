[![Validation CI](https://github.com/ansible-content-lab/aoc.controller_demo_config/actions/workflows/on-push.yml/badge.svg)](https://github.com/ansible-content-lab/aoc.controller_demo_config/actions/workflows/on-push.yml)

# Ansible on Clouds Controller Setup and Configuration

This collection contains playbooks and roles that demonstrate using configuration-as-code to declare the setup and configuration for Ansible Automation Controller -- seeding opinionated automation content into the platform.  This approach to Ansible Automation Controller configuration provides an easy-to-read and manage deployment model and keeps your Ansible Automation Controller configuration portable between different AAP deployments.

## Included Content

<!--start collection content-->
### Roles

Click on the role name to be directed to the README specifically for that role.

| Name                                                                                                                                            | Description                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| [aoc.controller_demo_config.controller](https://github.com/ansible-content-lab/aoc.controller_demo_config/blob/main/roles/controller/README.md) | Role that deploys a demo configuration of job templates, projects, inventories, etc. |

### Playbooks

| Name                         | Role(s) Used                               | Description                                               |
| ---------------------------- | ------------------------------------------ | --------------------------------------------------------- |
| `playbook_configure_aap.yml` | `aoc.ansible_controller_config.controller` | A playbook that runs the AAP on Azure configuration role. |

#### Running Configure AAP Azure

Run the `aoc.playbook_configure_aap.yml` playbook with `ansible_navigator` from the root directory of this repository.  

The example below sets environment variables and extra vars in different ways to demonstrate how you can pass such variables.  Take not that in the case of the file lookup examples, those files will need to reside from within the Execution Environment container.  Because of that, the `.ssh` folder is mapped from localhost into the EE.

```bash
ansible-navigator run playbook_configure_aap.yml \
--pae false \
--mode stdout \
--ee true \
--eei quay.io/scottharwell/cloud-ee:latest \
--senv "CONTROLLER_HOST=controller.112233445566.ansiblecloud.redhat.com" \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_SECRET \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD \
--eev $HOME/.ssh:/home/runner/.ssh
```

## Installation and Usage

### Installing the Collection with the Ansible Galaxy CLI

Before using the this collection, you need to install it with the Ansible Galaxy CLI:

`ansible-galaxy collection install git+https://github.com/ansible-content-lab/aoc.controller_demo_config.git`

You can also include it in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: https://github.com/ansible-content-lab/aoc.controller_demo_config
    type: git
    version: main
```

### Playbook Use

This collection is not available on Ansible Galaxy, so you will need to have it available for a playbook either through an Execution Environment or manual installation.  Once available, referencing the role happens just like any other Ansible role.

```yaml
- hosts: localhost
  roles:
    - role: aoc.ansible_controller_config.controller
```

## License

GNU General Public License v3.0 or later

See [LICENSE](https://github.com/ansible-content-lab/aoc.controller_demo_config/blob/main/LICENSE) to see the full text.

## Author

This collection was originally written by Scott Harwell and Hicham Mourad from the Ansible team at Red Hat.
