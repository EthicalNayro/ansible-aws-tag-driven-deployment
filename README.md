# AWS EC2 Dynamic Provisioning with Ansible (Tag-Driven Deployment)

![Ansible](https://img.shields.io/badge/Ansible-E25A1C?style=for-the-badge&logo=ansible&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

An automated Infrastructure-as-Code (IaC) solution using **Ansible Dynamic Inventory** (`aws_ec2` plugin) to discover, configure, and manage AWS EC2 instances dynamically based on **AWS Tags**.

---

## 📐 Architecture & Flow

Instead of hardcoding static IP addresses, this project queries the AWS EC2 API in real-time, filters target instances by `Owner`, and applies system configurations dynamically based on instance metadata:
```
                         ┌──────────────────────┐
                         │ AWS EC2 Instances    │
                         └──────────┬───────────┘
                                    │
                   1. Discover Hosts by Tag Filter (Owner)
                                    ▼
       ┌────────────────────────────────────────────────────────┐
       │  Ansible Control Node                                  │
       │  - Dynamic Inventory: aws_ec2.yml                      │
       │  - Playbook: setup_servers.yml                         │
       └────────────────────────────┬───────────────────────────┘
                                    │
                   2. Apply Configuration over SSH
                                    ▼
           ┌────────────────────────┴────────────────────────┐
           │                                                 │
           ▼                                                 ▼
    ┌─────────────────────────┐       ┌─────────────────────────┐
    │ Worker: Nginx Web       │       │ Worker: MySQL DB        │
    │ - Hostname Updated      │       │ - Hostname Updated      │
    │ - Nginx Installed       │       │ - MySQL Installed       │
    │ - Weekly Cron Scheduled │       │ - Weekly Cron Scheduled │
    └─────────────────────────┘       └─────────────────────────┘
```
