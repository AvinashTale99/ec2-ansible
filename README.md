
# 🛠️ Ansible Setup on AWS EC2 Instances

## 📘 Overview  
This guide walks you through setting up an Ansible control node on an AWS EC2 instance and connecting it to two managed EC2 hosts for automated configuration management.

---

## ☁️ 1. EC2 Instance Setup

### 🔹 Launch 3 EC2 Instances
- **AMI**: Amazon Linux 2023  
- **Network**: Same VPC and Subnet

### 🖥️ Instance Types

| Role                | Instance Type    |
|---------------------|------------------|
| Ansible Control Node| `t3.medium` _(Start with `t3.micro`)_ |
| Managed Host 1      | `t3.micro`       |
| Managed Host 2      | `t3.micro`       |

### 🔐 SSH Key Pair
Use a key pair named: `ansible.pem`

---

## ⚙️ 2. Setup on the Control Node

### 🖥️ Connect to Control Node

```bash
ssh -i "ansible.pem" ec2-user@<Control_Node_Public_IP>
```

### 🛠️ Install Dependencies

```bash
sudo yum update -y
sudo yum install python3-pip tree -y

python3 --version
pip3 --version
```

### 📦 Install Ansible

```bash
sudo pip3 install ansible
```

**Optional alternative:**

```bash
sudo yum search ansible
sudo yum install ansible.noarch
```

```bash
ansible --version
```

---

## 📁 3. Ansible Project Structure

Recommended directory layout on the control node (`/etc/ansible` or your project directory):

```bash
.
├── ansible.cfg
├── aws
│   └── ansible.pem         # SSH private key
├── inventory
│   └── hosts               # Inventory file with EC2 IPs
├── playbooks              # Ansible playbooks
└── roles                  # Ansible roles
```

---

## ⚙️ 4. `ansible.cfg`

Create an `ansible.cfg` file in the root of your project:

```ini
[defaults]
inventory = ./inventory
remote_user = ec2-user
private_key_file = aws/ansible.pem
host_key_checking = False
retry_files_enabled = False

[privilege_escalation]
become = true
become_method = sudo
become_user = root
become_ask_pass = false
```

---

## 📇 5. Inventory File

Create the file `inventory/hosts` with your EC2 public IPs:

```ini
[all]
3.110.103.199
13.233.144.213
```

**Optional grouping:**

```ini
[webservers]
3.110.103.199
13.233.144.213
```

---

## 🔎 6. Test Ansible Connection

Run the ping module to test SSH connectivity:

```bash
sudo ansible -m ping all
```

### ✅ Expected Output:

```txt
3.110.103.199 | SUCCESS => {...}
13.233.144.213| SUCCESS => {...}
```

---

## 🛡️ Notes

- Make sure the **security group** of all instances allows inbound SSH (port **22**) from your IP or your VPC.
- Ensure the **SSH private key** has correct permissions:

```bash
chmod 400 aws/ansible.pem
```
## 👤 Author

## 👤 Author

**Avinash Tale**  
GitHub: [AvinashTale99](https://github.com/AvinashTale99)
Docker Hub: [avinashtale99](https://hub.docker.com/u/avinashtale99)

---

