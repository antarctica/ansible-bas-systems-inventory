# BAS Systems Inventory (`bas-systems-inventory`)

Integrates hosts with the BAS Systems Inventory

**This role is developed outside of the BAS Ansible Roles Collection**

### Intended Audience

This role is designed to meet the needs of BAS projects only, it is not intended to be a generic role.
Consequently this role may not suit the needs of other users or teams. This is not considered a limitation.

This role is made available publicly to ease distribution and under our commitment to open sourcing software wherever
possible.

BAS also produces a number of roles, which are intended to be generic, under the BAS Ansible Roles Collection (BARC).
See [here](https://galaxy.ansible.com/BARC/) for available roles, 
and [here](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt) for a general overview and policies.

## Overview

* Based on a given System (project), determines the Compute Resource this host represents and the System Instance this
belongs to
* Updates the associated Compute Resource with details from the host
* Records key identifiers about the associated Compute Resource and System Instance as Ansible local facts

## Dependencies

* None

## Requirements

* The `bas_systems_inventory_system_reference` variable **MUST** be set to the identifier of a System within the 
Systems Inventory

It is recommended to set this in a *group_var* which applies to all hosts (i.e. `group_vars/all.yml`).

* The `bas_systems_inventory_instance_environment` variable **MUST** be set to a valid System Instance environment

For `development-local` environments, it is recommended to set this within a suitable *group_var* which applies to all
relevant hosts (i.e. `group_vars/vagrant.yml` if using Vagrant to manage local VMs).

For other environments, this information should come from host meta-data (e.g. Tags if using AWS EC2) or through 
suitable *group_vars*.

## Limitations

* None

## Usage

Include this role as normal, it should be listed after any BARC role(s), if used, and before other BAS specific roles.

### Local facts

This role will set a number of Ansible local facts under the `bas_systems_inventory` namespace:

* `ansible_local.bas_systems_inventory.compute_resource.id` - The ID of the associated Compute Resource for this host
* `ansible_local.bas_systems_inventory.compute_resource.manager` - The Manager of this host for the Compute Resource
* `ansible_local.bas_systems_inventory.compute_resource.provider` - The Provider of this host for the Compute Resource
* `ansible_local.bas_systems_inventory.compute_resource.system_image_name` - Host system image name
* `ansible_local.bas_systems_inventory.compute_resource.system_image_version` - Host system image version

* `ansible_local.bas_systems_inventory.system_instance.id` - The ID of the associated System Instance for this host
* `ansible_local.bas_systems_inventory.system_instance.environment` - The environment of the same System Instance

These facts will be updated each time this role is applied, however most are designed to outlive the lifespan of 
specific instances of a Compute Resource and so will effectively not change once set.

### Typical playbook

```yaml
---

- name: link hosts with the BAS Systems Inventory
  hosts: all
  become: yes
  vars: []
  roles:
    - antarctica.bas-systems-inventory
```

### Tags

Tags are not used in this role.

### Variables

#### *bas_systems_inventory_system_reference*

* **MUST** be specified
* Specifies the System (project) this host ultimately belongs to in the Systems Inventory
* Values **MUST** be a valid reference to a System as determined by the BAS Systems Inventory
* Example: `pristine` 

#### *bas_systems_inventory_instance_environment*

* **MUST** be specified
* Specifies the environment of the System Instance this host ultimately to in the Systems Inventory
* Values **MUST** be a valid environment for a System Instance as determined by the BAS Systems Inventory
* Example: `development-local`

#### *bas_systems_inventory_host_details*

* **SHOULD NOT** be specified
* Array of items used for updating information about this hosts corresponding Compute Resource in the Systems Inventory
* The value of this variable **MUST** be compatible with the Systems Inventory via its relevant endpoint
* For compatibility with the Systems Inventory this variable **SHOULD NOT** be changed

## Developing

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the 
[BAS Web Services Forum](https://jira.ceh.ac.uk/projects/WSF) (WSF) project on Jira.

This service is currently only available to BAS or NERC staff, although external collaborators can be added on request.
See our contributing policy for more information.

### Source code

All changes should be committed, via pull request, to the canonical repository, which for this project is:

`ssh://git@stash.ceh.ac.uk:7999/wsf/ansible-bas-systems-inventory.git`

A mirror of this repository is maintained on GitHub. Changes are automatically pushed from the canonical repository to
this mirror, in a one-way process.

`git@github.com:antarctica/ansible-bas-systems-inventory.git`

Note: The canonical repository is only accessible within the NERC firewall. External collaborators, please make pull 
requests against the mirrored GitHub repository and these will be merged as appropriate.

### Contributing policy

This project welcomes contributions, see `CONTRIBUTING.md` for our general policy.

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) 
workflow is used to manage the development of this project:

* Discrete changes should be made within feature branches, created from and merged back into develop 
(where small changes may be made directly)
* When ready to release a set of features/changes, create a release branch from develop, update documentation as 
required and merge into master with a tagged, semantic version (e.g. v1.2.3)
* After each release, the master branch should be merged with develop to restart the process
* High impact bugs can be addressed in hotfix branches, created from and merged into master (then develop) directly

## Licence

Copyright 2016 NERC BAS.

Unless stated otherwise, all documentation is licensed under the Open Government License - version 3. All code is
licensed under the MIT license.

Copies of these licenses are included within this role.
