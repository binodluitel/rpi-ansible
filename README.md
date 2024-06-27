# Ansible Setup

The directory structure follows the alternative directory layout described in [ansible tips and tricks sample setup](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html#alternative-directory-layout).

## Basic Commands

### List all available hosts

```bash
ansible all -i ./inventories --list-hosts
```

### List all available tags

```bash
ansible-playbook -i ./inventories/rpis ./site.yaml --list-tags
```

### Ping host(s) - master

```bash
ansible-playbook -i ./inventories/rpis ./masters.yaml --tags ping
```

## Deploy to Raspberry Pi

The ansible script is tested on Raspberry Pi 4 with at least 4GB RAM.
The script hasn't been tested on any other Raspberry Pi models.

### Raspberry Pi Setup

1. Install Raspberry Pi OS on the SD card.
2. Make sure to enable SSH and set up the network.

### Deploy to Master node

```bash
ansible-playbook -i ./inventories/rpis ./masters.yaml
```
