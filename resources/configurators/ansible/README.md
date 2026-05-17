# Ansible configurator

- [Ansible configurator](#ansible-configurator)
  - [Versions](#versions)
  - [Usage](#usage)
  - [Points to consider](#points-to-consider)

The FortiGate best practices baseline can be deployed using Ansible. Not all topics from the baseline can be configured because of limitations or because of missing information.

The various sections of the baseline are in their own task files.

## Versions

* FortiOS: 7.6.6
* Ansible: 2.16.3
* Ansible fortinet.fortios collection: 2.5.1
* Python: 3.12.3

## Usage

**Important:** Before using the configurator, make sure to carefully read the [**main.yml**](./roles/fgt-baseline/vars/main.yml) variable file, because it holds all the important information regarding the configuration, and if variables are configured incorrectly, you can lock yourself out of the FortiGate or cause an outage.

The configurator is provided as a role, but the tasks and required variable file can be used outside of a role. After copying the relevant files, edit the [**main.yml**](./roles/fgt-baseline/vars/main.yml) file and supply your own variable values where necessary.

The configurator was tested using the API method with an access token. Consult the documentation on [how to create an API user and generate a token](https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/399023/rest-api-administrator).

An [example inventory](example_inventory.ini) as well as an [example playbook](configure_baseline.yml) are provided.

## Points to consider

* I am not liable for any issues you experience when using the configurator.
* Settings that are currently not implemented or important notices are displayed after the configurator runs and can be seen in the [**main.yml**](./roles/fgt-baseline/vars/main.yml) in the `not_implemented_settings` list.