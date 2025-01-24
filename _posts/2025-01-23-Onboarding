---
title: Onboarding a New System
categories: [Guides]
tags: [Onboarding, VM, Bare Metal, Setup, Ansible]
---

## Onboarding a New System

Once the system boots, gain console access (via ProxMox Console, SSH, or phyiscal interface), then run the following commands:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
sudo apt install git -y
sudo apt install ansible -y
sudo echo "ANSIBLE_VAULT_PASSWORD" > /home/michel/.vault_pass.txt
sudo ansible-pull -U https://github.com/MichelfrancisBustillos/ansible_pull.git --vault-password-file /home/michel/.vault_pass.txt
```

This will fully update the system and set it up with Ansible Pull.
The Ansible playbook will:

- Install Tools:
  - BatCat
  - Micro
  - Pipx
- Install Agents:
  - Tailscale
  - NetData
  - Glances
- Configure Environment:
  - Replace .bashrc and .bash_aliases
  - Enable Pushover notifications via email (postfix)
  - Enable unatteneded updates
  - Setup default user accounts
  - Setup regular checks for Ansible playbooks updates
