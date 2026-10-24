# Ansible Linux Automation

A practical Ansible project for automating essential Linux server tasks, hardening SSH access, configuring a firewall, creating scheduled backups, and deploying a simple web service.

This repository is designed to demonstrate real-world infrastructure automation skills using Infrastructure as Code (IaC) principles.

## Project Goal

This project automates a basic Linux server setup by:
- updating and maintaining system packages
- configuring the base system
- hardening SSH access
- enabling firewall rules
- creating automated backups
- deploying a basic NGINX landing page

## Why This Project Matters

This project reflects common responsibilities in Linux system administration and DevOps:
- keeping systems consistent and repeatable
- reducing manual server setup work
- improving security posture
- documenting infrastructure changes in version control
- automating routine maintenance tasks

## Repository Structure

```text
ansible/
├── README.md
├── playbook.yml
├── start.sh
├── inventory/
│   └── local.yml
├── roles/
│   ├── base/
│   │   └── tasks/
│   │       └── main.yml
│   ├── sshd/
│   │   └── tasks/
│   │       └── main.yml
│   ├── firewall/
│   │   └── tasks/
│   │       └── main.yml
│   ├── backup/
│   │   └── tasks/
│   │       └── main.yml
│   └── web/
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
│           └── index.html.j2
└──
```

## Prerequisites

- Ubuntu or Debian-based Linux machine
- Ansible installed on the control machine
- SSH access to the target host
- sudo privileges on the remote server
- SSH key-based authentication configured
- Python installed on the target host

Install Ansible:

```bash
sudo apt update
sudo apt install ansible -y
```

## Quick Start

1. Clone the repository

```bash
git clone https://github.com/TAHA5100/ansible.git
cd ansible
```

2. Update the inventory file

```yaml
all:
  children:
    linux_servers:
      hosts:
        web01:
          ansible_host: 192.168.56.10
          ansible_user: admin
          ansible_ssh_private_key_file: ~/.ssh/id_rsa
```

3. Validate the playbook

```bash
ansible-playbook --syntax-check playbook.yml
```

4. Run the playbook

```bash
ansible-playbook -i inventory/local.yml playbook.yml
```

5. Verify the result

```bash
sudo systemctl status nginx
sudo ufw status
df -h
free -h
```

## Main Playbook

```yaml
---
- name: Configure Linux server baseline
  hosts: linux_servers
  become: true
  roles:
    - base
    - sshd
    - firewall
    - backup
    - web
```

## Included Roles

### Base role
Performs the initial system setup:
- update package cache
- upgrade installed packages
- install base utilities such as curl, git, jq, htop, rsync, cron, and tree
- configure timezone
- set hostname

### SSHD role
Hardens SSH access:
- disables password authentication
- disables root SSH login
- enables public key authentication
- restarts the SSH service

### Firewall role
Configures the server firewall:
- installs UFW
- allows SSH traffic
- allows HTTP traffic on port 80
- enables UFW with a default deny policy

### Backup role
Creates automated backup functionality:
- creates a backup directory
- writes a backup script
- schedules a cron job to run nightly backups

### Web role
Deploys a simple web server:
- installs NGINX
- creates the web root
- deploys a landing page using a template
- ensures the NGINX service is running

## Validation

Run these checks before and after deployment:

```bash
ansible-playbook --syntax-check playbook.yml
ansible-playbook -i inventory/local.yml playbook.yml --check
ansible-playbook -i inventory/local.yml playbook.yml
```

## Useful Notes

- This repository is intentionally focused on a simple Linux server baseline.
- The role structure is modular and easy to extend.
- You can add more roles later for monitoring, Docker, Kubernetes, log collection, or application deployment.
- The project is suitable as a learning project, portfolio project, or foundation for a larger Ansible setup.

## Future Improvements

Potential upgrades for this project include:
- adding a monitoring role
- using `group_vars` for centralized configuration
- separating inventory into `dev` and `prod`
- adding `ansible.cfg` for standard configuration
- adding linting and CI checks for better automation quality

This project is a solid beginner-to-intermediate Ansible setup that demonstrates infrastructure automation, security hardening, and deployment fundamentals in a practical way.
