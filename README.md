# 🚀 Automated AWS Infrastructure & Deployment with Ansible

Welcome to the Automated AWS Server Configuration project! This repository contains a fully automated Infrastructure-as-Code (IaC) solution for provisioning, configuring, and monitoring cloud servers.

---

## 1. Project Overview
This project demonstrates how to build a practical DevOps environment in AWS where Ansible automatically configures and manages multiple Ubuntu servers[cite: 6]. It replaces repetitive manual tasks with automated code, deploying a secure, monitored, and container-ready web architecture[cite: 6].

## 2. Problem Statement
Manually configuring servers, installing packages, configuring applications, and setting up monitoring is time-consuming, prone to human error, and difficult to scale[cite: 6]. Inconsistent environments lead to deployment failures and security vulnerabilities.

## 3. Objectives
* Replace manual server configuration with automated Ansible playbooks[cite: 6].
* Standardize infrastructure using modular Ansible Roles[cite: 6].
* Securely manage sensitive credentials (like API keys) using Ansible Vault[cite: 6].
* Implement automated infrastructure monitoring to detect issues early[cite: 6].
* Provide a beginner-friendly foundation for learning core DevOps practices[cite: 6].

## 4. Architecture Diagram
    [Ansible Controller] (Your Local Machine)
             | 
             | (SSH via port 22)
             v
      +-----------------------+
      |    AWS Cloud (VPC)    |
      |                       |
      |  +-----+     +-----+  |
      |  | Web |     | App |  |
      |  +-----+     +-----+  |
      |     |           |     |
      |  [Nginx]    [Docker]  |
      |     |           |     |
      +-----|-----------|-----+
            v           v
      [New Relic Observability]

*(Note: A Database server layer is currently in development for Phase 11)*[cite: 6]

## 5. Technology Stack
* **Cloud Provider:** AWS (EC2)[cite: 6]
* **OS:** Ubuntu Linux (24.04 Noble)[cite: 6]
* **Configuration Management:** Ansible[cite: 6]
* **Web Server:** Nginx[cite: 6]
* **Containerization:** Docker[cite: 6]
* **Observability:** New Relic[cite: 6]
* **Version Control:** Git & GitHub[cite: 6]

## 6. AWS Architecture
The project currently utilizes two AWS EC2 `t2.micro` instances[cite: 6]:
* **Web EC2:** Dedicated to routing traffic via Nginx[cite: 6].
* **App EC2:** Dedicated to running application workloads via Docker[cite: 6].
* **Security Groups:** Configured to allow SSH (Port 22) for Ansible, and HTTP (Port 80) for web traffic.

## 7. Ansible Architecture
Ansible operates on a push-based model. The local machine acts as the **Controller**, utilizing SSH to enforce the desired state on the AWS **Managed Nodes**[cite: 6]. We utilize variables, facts, handlers, and Jinja2 templates to make the configuration dynamic and reusable[cite: 6].

## 8. Repository Structure
ansible-aws-devops-project/
├── ansible.cfg              # Ansible configuration settings
├── inventory/               # Defines the AWS host IP addresses
├── playbooks/
│   └── site.yml             # The master playbook bridging all roles
├── roles/
│   ├── common/              # Baseline server updates and packages
│   ├── docker/              # Docker installation and permissions
│   ├── nginx/               # Web server installation and templating
│   └── newrelic/            # Infrastructure agent and GPG keys
└── vault/                   # Encrypted API keys and passwords

## 9. Setup Instructions
1. **Clone the repository:**
   `git clone https://github.com/YOUR-USERNAME/ansible-aws-devops-project.git`
2. **Update Inventory:** 
   Add your AWS EC2 Public IPs to the `inventory` file.
3. **Configure SSH:** 
   Ensure your AWS `.pem` key is downloaded and its path is referenced in `ansible.cfg`.
4. **Create the Vault:**
   Run `ansible-vault create vault/secrets.yml` to securely store your New Relic License key.

## 10. Deployment Instructions
To deploy the entire infrastructure from scratch, simply run the master playbook:
ansible-playbook playbooks/site.yml

Because Ansible is idempotent, you can run this command safely as many times as you want. It will only make changes if the servers drift from the desired state[cite: 6].

## 11. Security Considerations
* **No hardcoded credentials:** Passwords and API keys are strictly kept out of standard configuration files[cite: 6].
* **.gitignore enforcement:** AWS `.pem` private keys are explicitly ignored by Git to prevent accidental leakage to public repositories[cite: 6].
* **Least privilege:** Services run with specific permissions, and Ubuntu standard user accounts are added to the Docker group to avoid running containers as root unnecessarily[cite: 6].

## 12. Secrets Management
All sensitive data is encrypted using **Ansible Vault**[cite: 6]. The vault file is unlocked at runtime by the Ansible Controller using a master password, injecting the secrets into memory without ever writing them to disk in plain text.

## 13. New Relic Monitoring
This project fully automates the installation of the New Relic Infrastructure agent[cite: 6]. It securely handles GPG key verification, dynamically injects the INGEST - LICENSE key via Ansible Vault, and streams live CPU, memory, and disk metrics to the New Relic SaaS dashboard[cite: 6].

## 14. Failure Testing
To prove the resilience of this automation, you can run the following failure scenarios[cite: 6]:
* **Test 1:** Manually stop the Nginx service on the Web server[cite: 6]. Run the playbook. Ansible will detect the stopped state and automatically restore the service[cite: 6].
* **Test 2:** Uninstall Docker from the App server[cite: 6]. Run the playbook. Ansible will completely reinstall and reconfigure the container runtime[cite: 6].

## 15. Troubleshooting
* **SSH Connection Refused:** Verify your AWS Security Group allows inbound traffic on Port 22 from your current IP address.
* **Vault Decryption Failed:** Ensure you are entering the correct master password when executing the playbook.
* **New Relic 401 Unauthorized:** Check your `vault/secrets.yml` file to ensure you are using an `INGEST - LICENSE` key, not a User API key.

## 16. Cost-Control Instructions
This environment is designed for the AWS Free Tier[cite: 6]. However, running multiple EC2 instances 24/7 will quickly drain your monthly 750 free hours. 
**Important:** Always stop your EC2 instances from the AWS Management Console when you are done working for the day[cite: 6]. Do not terminate them unless you are ready to destroy the environment entirely.

## 17. Lessons Learned
Through this project, I gained hands-on experience troubleshooting strict Linux package manager changes (Ubuntu 24.04 `.asc` keys), managing AWS networking, structuring reusable YAML automation code, and securing infrastructure pipelines against accidental credential leaks.
