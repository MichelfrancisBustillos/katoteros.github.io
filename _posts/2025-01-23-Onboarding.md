---
title: Onboarding a New System
categories: [Guides]
tags: [onboarding, vm, bare metal, setup, ansible]
---

## Onboarding a New System
Onboarding a new VM, LXC Container, or bare metal server using Ansible Pull.

1. Log into the VM or bare metal server as the `root` user:

    ```bash
    apt update && apt upgrade -y && apt install git ansible -y
    scp michel@<VMHost>:/home/michel/.vault_pass.txt .vault_pass.txt
    ansible-pull -U https://github.com/MichelfrancisBustillos/ansible_pull.git --vault-password-file .vault_pass.txt
    ```
2. Log in as the default user `michel`:
    **Note:** If working with an LXC Container, run the AppArmor Error fix first
    ```bash
    scp michel@<VMHost>:/home/michel/.vault_pass.txt /home/michel/.vault_pass.txt
    ansible-pull -U https://github.com/MichelfrancisBustillos/ansible_pull.git --vault-password-file /home/michel/.vault_pass.txt
    ```


This will fully update the system and set it up with Ansible Pull.
The Ansible playbook will:

- Install Tools:
  - BatCat
  - Micro
  - Pipx
  - Docker
- Install Agents:
  - Tailscale
  - Glances
  - Portainer
  - Watchdog
- Configure Environment:
  - Replace .bashrc and .bash_aliases
  - Enable Pushover notifications via email (postfix)
  - Enable unattended updates
  - Setup default user accounts
  - Setup regular checks for Ansible playbooks updates
