# ansible-playbooks

ansible playbook is a collection of ansible code that will be executed by ansible to configure the server.

## Prerequisites
 - Control node (e.g local computer or any VPS)
 - Remote Node (e.g VPS)

> note : **control node** is where you will install ansible and execute the ansible-playbooks. I use my local computer for control node.

> note : Remote node

## Installation

- make sure ansible is installed in your control node. read [ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html) doc to install ansible.

- make sure you can connect using ssh key based (passwordless) from control node to remote host as **root** user. read [here](https://www.redhat.com/sysadmin/passwordless-ssh) if you have not configure it.

## License
[GNU GPL 3.0](https://github.com/dekiakbar/ansible-playbooks/blob/master/LICENSE)