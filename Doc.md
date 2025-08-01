# 📘 Infrastructure Setup & Configuration with Ansible on AWS

## 🧾 Overview

This guide outlines the process shared by **Atul Kamble** during the infrastructure configuration session. It walks through launching EC2 instances, installing and configuring Ansible, and handling Docker + Jenkins integration for deployment.

---

## 📌 Prerequisites

* AWS account access
* Basic Linux knowledge
* SSH key pair
* IAM role with EC2 & S3 permissions (if needed)
* Python 3.x and pip
* Virtual environment support (`venv`)

---

## 🛠️ Step 1: Launch EC2 Instances

### ✅ Instance Configuration

| Setting        | Value                    |
| -------------- | ------------------------ |
| AMI            | Amazon Linux 2023        |
| Instance Type  | t3.medium                |
| Instances      | 3                        |
| Tags           | Host1, Host2, Encumbered |
| Security Group | Public IP + SSH Access   |

### 💡 Notes

* Maintain **consistent instance count**
* Ensure **public IPs** are assigned
* Reserve names: **Host 1**, **Host 2**, **Encumbered Server**

---

## 🧱 Step 2: Install Ansible

> Do **not** run Ansible as `root`. Use a virtual environment to avoid permission issues.

### 🔧 Commands

```bash
sudo yum update -y
sudo yum install python3 -y
python3 -m venv ansible_env
source ansible_env/bin/activate
pip install --upgrade pip
pip install ansible
```

> 🔍 Search for available Ansible packages if needed:

```bash
pip search ansible
```

---

## 📁 Step 3: Set Up Project Directory

```bash
cd /etc
sudo mkdir -p ansible_project/{inventory,playbooks}
cd ansible_project
```

### Desired Structure

```
ansible_project/
├── ansible.cfg
├── inventory/
│   └── hosts
├── playbooks/
│   └── setup.yml
```

---

## ⚙️ Step 4: Configure `ansible.cfg`

```bash
sudo nano ansible.cfg
```

### Sample Content

```ini
[defaults]
inventory = ./inventory/hosts
private_key_file = ~/.ssh/your-key.pem
host_key_checking = False
remote_user = ec2-user
```

---

## 📋 Step 5: Define Hosts (Inventory File)

```bash
nano inventory/hosts
```

### Example Content

```ini
[webservers]
host1 ansible_host=YOUR.HOST1.PUBLIC.IP
host2 ansible_host=YOUR.HOST2.PUBLIC.IP

[db]
encumbered ansible_host=YOUR.ENCUMBERED.IP
```

---

## 📂 Step 6: Create Sample Playbook

```bash
nano playbooks/setup.yml
```

```yaml
- name: Test Connectivity
  hosts: all
  tasks:
    - name: Ping all hosts
      ping:
```

### Run:

```bash
ansible-playbook playbooks/setup.yml
```

---

## 🐛 Troubleshooting

### 🔸 SSH Key Issues

* **Error**: *Permission denied (publickey)*
* ✅ Fix:

  ```bash
  chmod 400 ~/.ssh/your-key.pem
  ```

### 🔸 Host Key Checking Warning

* *"Permanently added the host..."*
* ✅ Disable in `ansible.cfg`:
  `host_key_checking = False`

---

## 🐳 Docker Push Issue (via Jenkins)

* **Problem**: Access denied to Docker registry
* **Identified Cause**: Incorrect username (`h042`)
* ✅ Fix:

  * Check Docker login credentials
  * Validate Jenkins credentials configuration
  * Re-authenticate using:

    ```bash
    docker login
    ```

---

## 🚀 Scaling Strategy

### Vertical Scaling:

* Upgrade instance types for performance (e.g., `t3.medium` → `t3.large`)

### Horizontal Scaling:

* Maintain consistent instance count and naming conventions

> Instances can be fully stopped and reconfigured.

---

## ✅ Confirmation Steps

* Ping all hosts via Ansible
* Verify directory structure
* Ensure Docker push and Jenkins integration are error-free

---

## 👥 Contributors

* **Atul Kamble** – Lead Configuration & Troubleshooting
* **Avinash Tale** – Assisted in Setup & SSH Config
* **Harshal Mali** – Helped Resolve Docker Login Issue

---

