🛠️ Ansible Setup on AWS EC2 Instances
📘 Overview
This guide walks you through setting up an Ansible control node on an AWS EC2 instance and connecting it to two managed EC2 hosts for automated configuration management.

☁️ 1. EC2 Instance Setup
🔹 Launch 3 EC2 Instances
AMI: Amazon Linux 2023

Network: Same VPC and Subnet

🖥️ Instance Types
Role	Instance Type
Ansible Control Node	t3.medium (Start with t3.micro)
Managed Host 1	t3.micro
Managed Host 2	t3.micro

🔐 SSH Key Pair
Use a key pair named: ansible.pem

⚙️ 2. Setup on the Control Node
🖥️ Connect to Control Node
SSH into the control node:

bash
Copy
Edit
ssh -i "ansible.pem" ec2-user@<Control_Node_Public_IP>
🛠️ Install Dependencies
bash
Copy
Edit
sudo yum update -y
sudo yum install python3-pip tree -y

python3 --version
pip3 --version

# Install Ansible
sudo pip3 install ansible

# Optional alternative:
sudo yum search ansible
sudo yum install ansible.noarch

ansible --version
📁 3. Ansible Project Structure
Recommended directory layout on the control node (/etc/ansible or your project directory):

bash
Copy
Edit
.
├── ansible.cfg
├── aws
│   └── ansible.pem          # SSH private key
├── inventory
│   └── hosts                # Inventory file with EC2 IPs
├── playbooks                # Ansible playbooks
└── roles                    # Ansible roles
⚙️ 4. ansible.cfg
Create an ansible.cfg file in the root of your project:

ini
Copy
Edit
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
📇 5. Inventory File
Create the file inventory/hosts with your EC2 public IPs:

ini
Copy
Edit
[all]
3.110.103.199 
52.90.194.255
Optional grouping:

ini
Copy
Edit
[webservers]
174.129.61.157
13.233.144.213
🔎 6. Test Ansible Connection
Run the ping module to test SSH connectivity:

bash
Copy
Edit
sudo ansible -m ping all
✅ Expected Output:
text
Copy
Edit

3.110.103.199  | SUCCESS => {...}
13.233.144.213 | SUCCESS => {...}
🛡️ Notes
Make sure the security group of all instances allows inbound SSH (port 22) from your IP or your VPC.

Ensure the SSH private key has correct permissions:

bash
Copy
Edit
chmod 400 aws/ansible.pem
