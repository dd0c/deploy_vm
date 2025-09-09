# deploy_vm

An Ansible role for deploying virtual machines on Proxmox from templates.

## Features

- Clone VMs from Proxmox templates
- Configurable template ID, node, and storage
- Support for different operating systems
- Flexible variable configuration

## Requirements

- Ansible 2.9+
- `community.general` collection
- Access to Proxmox API

## Installation

1. Install the required Ansible collection:
```bash
ansible-galaxy collection install community.general
```

2. Clone or download this role to your roles directory

## Usage

### Basic Usage

Create a playbook to deploy a VM from template ID 999:

```yaml
---
- name: Deploy VM from Proxmox template 999
  hosts: proxmox_hosts
  become: yes
  vars:
    proxmox_template_id: "999"
    HostNameVar: "my-new-vm"
  roles:
    - deploy_vm
```

### Advanced Configuration

```yaml
---
- name: Deploy VM with custom configuration
  hosts: proxmox_hosts
  become: yes
  vars:
    # Proxmox configuration
    proxmox_node: "your-proxmox-node"
    proxmox_api_user: "root@pam"
    proxmox_api_password: "{{ vault_proxmox_password }}"
    proxmox_api_host: "proxmox.example.com"
    proxmox_template_id: "999"
    proxmox_storage: "local-zfs"
    
    # VM configuration
    HostNameVar: "production-server"
    
  roles:
    - deploy_vm
```

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `proxmox_node` | `hera` | Proxmox node name |
| `proxmox_api_user` | `root@pam` | Proxmox API user |
| `proxmox_api_password` | `{{ hera_root }}` | Proxmox API password |
| `proxmox_api_host` | `hera.doctert.net` | Proxmox API host |
| `proxmox_template_id` | `999` | Template ID to clone |
| `proxmox_storage` | `local-zfs` | Storage pool for the VM |
| `HostNameVar` | Required | Name for the new VM |

## Inventory

Create an inventory file with your Proxmox hosts:

```ini
[proxmox_hosts]
your-proxmox-node ansible_host=192.168.1.100

[proxmox_hosts:vars]
ansible_user=root
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

## Running the Playbook

```bash
ansible-playbook -i inventory.ini playbook.yml
```

## Security Notes

- Store sensitive passwords in Ansible Vault
- Use SSH keys for authentication
- Limit API user permissions when possible

## Examples

See `example-playbook.yml` and `inventory-example.ini` for complete examples.
