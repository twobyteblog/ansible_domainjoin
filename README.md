## Ansible Role: Domain Join for Debian Hosts

This Ansible role automates the process of joining a host to an Active Directory domain and configures centralized SSH authentication using the 'altSecurityIdentities' user attribute. The host is configured via the ```realm``` and ```sssd``` packages and joined using pre-provided domain credentials via a service account.

## Actions Performed

The role performs the following actions:

1. Installs required packages.
2. Joins host to the domain via ```realm```.
3. Updates ```sssd``` to allow ssh access via the ```altSecurityIdentities``` user attribute.
4. Enables home directory creation.
5. Configures sudo rights for Active Directory security group.

## Setup Instructions

### Clone Repository

Clone the project into the roles directory of your Ansible Controller.

```bash
git clone https://github.com/twobyteblog/ansible_domainjoin.git
```

### Variables

Within ```vars/main.yml``` fill in the following variables:

```bash
# Domain name.
domain: AD.TWOBYTE.BLOG

# Active Directory service account with domain-join rights.
join_account: service_account
join_account_password: password

# Location where created computer objects for newly joined hosts will be placed.
computer_ou: OU=Servers,DC=ad,DC=twobyte,DC=blog

# Active Directory security group with which members gain sudo rights over the host.
admin_group: mygroup
```

### Public Key

For every user, copy the user's public key to the ```altSecurityIdentities``` user attribute under their user account within Active Directory.

### Playbook

Create a playbook which runs this role. This role requires become privileges.

```
- hosts: all
  roles:
    - role: ansible_domainjoin
      become: yes
```