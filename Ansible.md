---
tags:
  - ToBeRefined
  - SysOps
---
---
# Ansible

## Inventory

- Multiple inventory can be used at the same time
- Dynamic inventory \- inventory can be fetched from the external source
- all, ungrouped \- default groups
- Different groups can represent different purposes \- where/what/when?
- Variables can be added to hosts directly in the inventory
- Can also add some aliases to hosts
- group variables in ini files are always interpreted as strings  \- when importing variables to the tasks we can declare vairable types
- groups can be grouped in groups and :children keyword can be used to access them,
- ansible is looking for group and host vars dirs in relation to the inventory file 
- you can organize variables into group\_vars and host\_vars
- Variables are flattened to host level when service is being run

## Vault

- Vault can encrypt inline variables in plaintext files or whole files
- You have to set password when encrypting
- Passwords can be saved in file or external storage
- You should store variable names and their values separately by adding [layer of indirection](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#keep-vaulted-variables-safely-visible) 
- You can pass vault\-id which will be added as a Header to encrypted content by ansible by default is not checking if correct vault\-id has been passed when decrypting \- only password matters 

## Roles

- Can be used to organize tasks, files, templates according to their function
- Can by applied to host or group in playbook
- Can pass some variables to it
- Made also for easily sharing some configuration

## Playbooks

- You can use register to save output of the task in the variable
- you can fetch some more vars by include\_vars and vars\_files

## Misc

- you can use ansbile localhost ansible.bultin.debug for local debugging
- Tasks can be tagged for easier control on what is executed
