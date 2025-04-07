# Ansible Role: Domain Join

This Ansible role automates the process of joining a host to an Active Directory domain and configures centralized SSH authentication using the 'altSecurityIdentities' user attribute. The host is configured via the ```realm``` and ```sssd``` packages and joined using pre-provided domain credentials via a service account.

This role will:

1. Install required packages on host.
2. Join the host to the domain, the computer object will be created within the OU path specified by the ```computer_ou``` variable.
3. Update ```sssd``` to allow ssh access via the 'altSecurityIdentities' user attribute.
4. Ensure home directories are created automatically on login.
5. Configure sudo rights to user accounts which are appart of the specified Active Directory security group.

# Requirements

This role requires root access to the host system. Specify it globally, or add it to your playbook (example below).

```
- hosts: all
  roles:
    - role: ansible_domainjoin
      become: yes
```

# Role Variables

Within ```vars/main.yml``` provide the following variables:

```bash
# Domain name.
domain: AD.INUNDATION.CA

# Active Directory service account with domain-join rights.
join_account: service_account
join_account_password: password

# Location where created computer objects for newly joined hosts will be placed.
computer_ou: OU=Servers,DC=ad,DC=inundation,DC=ca

# Active Directory security group with which members gain sudo rights over the host.
admin_group: mygroup
```

For each user account that will have the ability to login to the host via SSH, enter the user's public key under the ```altSecurityIdentities``` user attribute.

# Installation

To use this role, clone it into the directory containing your other Ansible roles.

```
git clone https://github.com/twobyteblog/ansible_domainjoin.git
```

Next, add it to a new or existing playbook.

```
- hosts: all
  roles:
    - role: ansible_domainjoin
      become: yes
```