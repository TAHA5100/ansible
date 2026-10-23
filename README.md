# Ansible

A practical Ansible project for automating essential Linux system administration tasks, enforcing security baselines, and managing core infrastructure in a repeatable and maintainable way.

This project is designed to demonstrate skills in:
- Linux system administration
- Server hardening
- Automation with Ansible
- Firewall and networking basics
- Backup and maintenance tasks
- Infrastructure as Code
- Service deployment and validation

## Project Goal

This project automates a typical Linux server setup used in real environments:
- update and maintain system packages
- create and manage users
- secure SSH access
- configure firewall rules
- monitor basic server health
- schedule automatic backups
- deploy a simple web service
- keep the environment repeatable and easy to manage

## Why This Project Matters

This is a strong portfolio project because it reflects real operational responsibilities in IT and infrastructure roles:
- keeping systems consistent and secure
- reducing manual tasks
- automating routine admin work
- making server configuration version-controlled
- improving reliability and accountability

## Repository Structure

```text
linux-automation/
├── README.md
├── ansible.cfg
├── playbook.yml
├── inventory/
│   └── local.yml
├── group_vars/
│   └── all.yml
├── roles/
│   ├── base/
│   │   └── tasks/main.yml
│   ├── users/
│   │   └── tasks/main.yml
│   ├── ssh/
│   │   └── tasks/main.yml
│   ├── firewall/
│   │   └── tasks/main.yml
│   ├── backup/
│   │   └── tasks/main.yml
│   ├── monitoring/
│   │   └── tasks/main.yml
│   └── web/
│       ├── tasks/main.yml
│       └── templates/index.html.j2
├── scripts/
│   └── backup.sh
├── .gitignore
├── .ansible-lint
└── .github/
    └── workflows/ansible-lint.yml
```
## Prerequisites

- Ubuntu or Debian Linux server
- Ansible installed on the control machine
- SSH access to target hosts
- sudo privileges on remote machines
- SSH key-based authentication configured
- Python

Install Ansible:

```bash
sudo apt update
sudo apt install ansible -y
```

## Quick Start

1. Clone the repository

```bash
git clone https://github.com/your-user/linux-automation.git
cd linux-automation
```

2. Update the inventory

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
    - users
    - ssh
    - firewall
    - monitoring
    - backup
    - web
```

## Included Tasks

### Base configuration
- system update
- package installation
- hostname and timezone configuration

### User and access control
- create non-root admin users
- configure sudo access
- secure SSH authentication

### Security
- disable password login
- disable root SSH access
- configure firewall rules

### Backup and monitoring
- create backup folders
- schedule automatic backups
- check disk and memory usage
- validate service status

### Web deployment
- install NGINX
- deploy a custom landing page
- ensure the service is running

## Validation

Run these checks before and after deployment:

```bash
ansible-playbook --syntax-check playbook.yml
ansible-playbook -i inventory/local.yml playbook.yml --check
ansible-playbook -i inventory/local.yml playbook.yml
```
