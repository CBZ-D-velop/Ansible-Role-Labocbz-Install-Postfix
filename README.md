# Ansible role: labocbz.install_postfix

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Git](https://img.shields.io/badge/Tech-git-orange)
![Tag: Cron](https://img.shields.io/badge/Tech-Cron-orange)
![Tag: Mailutils](https://img.shields.io/badge/Tech-Mailutils-orange)
![Tag: Colorized-logs](https://img.shields.io/badge/Tech-Colorized--logs-orange)
![Tag: Kbtin](https://img.shields.io/badge/Tech-Kbtin-orange)
![Tag: Postfix](https://img.shields.io/badge/Tech-Postfix-orange)

An Ansible role to install and configure Postfix on your host.

The Ansible role installs and configures Postfix, an email server, on a target system. It provides the ability to customize email aliases and also allows setting up a "mail sender" to act as an intermediary for sending emails.

With this role, you can easily define email aliases for specific recipients, such as "root," "hostmaster," "postmaster," "webmaster," and "www," each associated with corresponding email addresses.

Additionally, the role allows customization of the ESMTP (Extended Simple Mail Transfer Protocol) banner of the email server, which is displayed during communications with remote email clients.

In summary, the Postfix role simplifies the installation and configuration of a robust email server by offering easy customization of email aliases and the option to set up Postfix as a "mail sender" intermediary for sending emails. It provides a reliable solution for managing email communication within your organization.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
postfix_email_sender: true
postfix_email_sender_address: "in-v3.your.provider.com"
postfix_email_sender_port: 587
postfix_email_sender_api_key: "YOUR_API_KEY_TO_SEND_EMAILS"
postfix_email_relayhost: "[{{ postfix_email_sender_address }}]:{{ postfix_email_sender_port }}"

postfix_aliases_addresses:
  - alias: "root"
    address: "your.root.address@domain.tld"
  - alias: "hostmaster"
    address: "your.hostmaster.address@domain.tld"
  - alias: "postmaster"
    address: "your.postmaster.address@domain.tld"
  - alias: "webmaster"
    address: "your.webmaster.address@domain.tld"
  - alias: "www"
    address: "your.www.address@domain.tld"

postfix_esmtp_banner: "ESMTP Your Compagny"
postfix_mydestination: ""
postfix_mailname: "{{ ansible_fqdn }}"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_postfix_email_sender: true
inv_postfix_email_sender_address: "in-v3.my.provider.com"
inv_postfix_email_sender_api_key: "MY_API_KEY_TO_SEND_EMAILS"

inv_postfix_aliases_addresses:
  - alias: "root"
    address: "my.root.address@domain.tld"
  - alias: "hostmaster"
    address: "my.hostmaster.address@domain.tld"
  - alias: "postmaster"
    address: "my.postmaster.address@domain.tld"
  - alias: "webmaster"
    address: "my.webmaster.address@domain.tld"
  - alias: "www"
    address: "my.www.address@domain.tld"

inv_postfix_esmtp_banner: "ESMTP my Compagny"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_postfix"
    tags:
    - "labocbz.install_postfix"
    vars:
    postfix_email_sender: "{{ inv_postfix_email_sender }}"
    postfix_email_sender_address: "{{ inv_postfix_email_sender_address }}"
    postfix_email_sender_api_key: "{{ inv_postfix_email_sender_api_key }}"
    postfix_aliases_addresses: "{{ inv_postfix_aliases_addresses }}"
    postfix_esmtp_banner: "{{ inv_postfix_esmtp_banner }}"
    ansible.builtin.include_role:
    name: "labocbz.install_postfix"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
