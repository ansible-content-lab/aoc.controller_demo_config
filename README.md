# Ansible on Clouds Controller Setup and Configuration

This collection contains playbooks and roles that demonstrate using configuration-as-code to declare the setup and configuration for Ansible Automation Controller -- seeding opinionated automation content into the platform.  This approach to Ansible Automation Controller configuration provides an easy-to-read and manage deployment model and keeps your Ansible Automation Controller configuration portable between different AAP deployments.

## Included Content

### Roles

Click on the role name to be directed to the README specifically for that role.

| Name                                            | Description                                                                          |
| ----------------------------------------------- | ------------------------------------------------------------------------------------ |
| [lab.controller_demo_config.controller][readme] | Role that deploys a demo configuration of job templates, projects, inventories, etc. |

### Playbooks

| Name            | Role(s) Used                            | Description                                               |
| --------------- | --------------------------------------- | --------------------------------------------------------- |
| `configure_aap` | `lab.controller_demo_config.controller` | A playbook that runs the AAP on Azure configuration role. |

## Running the Automation

### Installing the Collection with the Ansible Galaxy CLI

Before using the this collection, you need to install it with the Ansible Galaxy CLI:

`ansible-galaxy collection install git+https://github.com/ansible-content-lab/lab.controller_demo_config.git`

You can also include it in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: https://github.com/ansible-content-lab/aoc.controller_demo_config
    type: git
    version: main
```

### Prepare Variables

You may apply variables from any standard Ansible method.  This example will assume that you have created a directory called `env` and that a `vars.yaml` file in that directory.

The example below sets some of the variables that are not passed in through running the ansible automation directly.  These variables are not secret or dynamically issued, so it can be easier to put these in a variables file.  See the role's [defaults.yml][defaults] file to view most of these types of variables that can be changed.

```yaml
---
controller_seed_lab_content: true
controller_session_cookie_age: 28800
controller_credentials_ssh_key_data: "{{ lookup('file', '~/.ssh/id_rsa') }}"
controller_projects_wait: true
```

### Prepare Inventory

You may apply an inventory from any standard Ansible method.  This example will assume that you have created a directory called `env` and that a `inventory` file in that directory.  This collection only automates against localhost since it communicates with Ansible Controller via APIs.

```ini
localhost ansible_connection=local
```

### Collection Installation

Install this collection via the Ansible galaxy CLI.

Run the `lab.configure_aap.yml` playbook with `ansible_navigator` from the root directory of this repository.

The example below sets environment variables and extra vars in different ways to demonstrate how you can pass such variables.  Take not that in the case of the file lookup examples, those files will need to reside from within the Execution Environment container.  Because of that, the `.ssh` folder is mapped from localhost into the EE.

```bash
ansible-navigator run lab.controller_demo_config.configure_aap \
-i env/inventory \
--pae false \
--mode stdout \
--lf /dev/null \
--ee true \
--pp always \
--eei quay.io/scottharwell/cloud-ee:latest \
--eev $HOME/.ssh:/home/runner/.ssh \
# required variables \
--senv "CONTROLLER_HOST=controller.112233445566.ansiblecloud.redhat.com" \
--senv "CONTROLLER_VERIFY_SSL=true" \
--penv CONTROLLER_USERNAME \
--penv CONTROLLER_PASSWORD \
--penv AZURE_TENANT \
--penv AZURE_SUBSCRIPTION_ID \
--penv AZURE_CLIENT_ID \
--penv AZURE_SECRET \
--penv AWS_ACCESS_KEY_ID \
--penv AWS_SECRET_ACCESS_KEY \
--penv AWS_SESSION_TOKEN \
--penv RED_HAT_ACCOUNT \
--penv RED_HAT_PASSWORD \
--extra-vars "@env/vars.yml" \
# optional variables \
--senv "SOCIAL_AUTH_AZUREAD_OAUTH2_KEY=key_no_quotes" \
--senv "SOCIAL_AUTH_AZUREAD_OAUTH2_SECRET=secret_no_quotes"
```

## License

GNU General Public License v3.0 or later

See [LICENSE](https://github.com/ansible-content-lab/aoc.controller_demo_config/blob/main/LICENSE) to see the full text.

## Author

This collection was originally written by Scott Harwell and Hicham Mourad from the Ansible team at Red Hat.

[readme]: https://github.com/ansible-content-lab/aoc.controller_demo_config/blob/main/roles/controller/README.md
[defaults]: https://github.com/ansible-content-lab/aoc.controller_demo_config/blob/main/roles/controller/defaults/main.yml
