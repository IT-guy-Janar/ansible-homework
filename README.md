Ansible playbook for configuring an Elasticsearch, Nginx and Samba infrastructure with Filebeat log shipping.

This playbook has been tested on:
* Rocky Linux 9

## Requirements

- [Git](https://git-scm.com/)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

## Setup

### 1. Clone the repository
```bash
git clone https://github.com/IT-guy-Janar/ansible-homework.git
cd ansible-homework
```

### 2. Create the Ansible Vault

Create the vault file with your credentials:
```bash
ansible-vault create group_vars/common/vault.yml
```

The vault must contain the following variables:
```yaml
# SSH credentials for srves
vault_srves_user: root
vault_srves_pass: your_password

# SSH credentials for srvweb
vault_srvweb_user: root
vault_srvweb_pass: your_password

# SSH credentials for srvfile
vault_srvfile_user: root
vault_srvfile_pass: your_password

# Elasticsearch superuser (elastic)
vault_elastic_pass: your_password

# Filebeat writer user
vault_filebeat_user: filebeatuser
vault_filebeat_pass: your_password

# Nginx web user
vault_web_user: webuser
vault_web_pass: your_password

# Samba user
vault_samba_user: sambauser
vault_samba_pass: your_password
```

### 3. Update the inventory

Edit `inventory/hosts.ini` with your server IP addresses:
```ini
[elasticsearch]
srves ansible_host=192.168.64.10

[web]
srvweb ansible_host=192.168.64.20

[fileserver]
srvfile ansible_host=192.168.64.30

[common:children]
elasticsearch
web
fileserver
```

### 4. Test connectivity
```bash
# Using vault password file
ansible all -i inventory -m ping --vault-password-file .vault_pass

# Using vault password prompt
ansible all -i inventory -m ping --ask-vault-pass
```

All hosts should respond with `pong` before proceeding.

## Running the playbook
```bash
# Using vault password file
ansible-playbook -i inventory site.yml --vault-password-file .vault_pass

# Using vault password prompt
ansible-playbook -i inventory site.yml --ask-vault-pass
```

## Running tests
```bash
# Using vault password file
ansible-playbook -i inventory test.yml --vault-password-file .vault_pass

# Using vault password prompt
ansible-playbook -i inventory test.yml --ask-vault-pass
```
