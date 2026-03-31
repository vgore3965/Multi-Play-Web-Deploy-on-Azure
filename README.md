
---

# 🚀 Multi-Play Web Deploy on Azure (Terraform + Ansible)

## 📌 Project Overview

In this project, I automated the deployment of a **static website on Azure Virtual Machines** using:

* **Terraform** → Infrastructure provisioning
* **Ansible** → Configuration management
* **Nginx** → Web server

This project demonstrates a complete **DevOps workflow** from infrastructure creation to application deployment.

---

## 📂 Project Structure

```bash
Multi-Play-Web-Deploy-on-Azure-Demo
├── ansible
│   ├── files
│   │   └── index.html
│   ├── inventory.ini
│   ├── README.md
│   └── site.yaml
└── terraform
    └── main.tf
```

---

## ⚙️ Prerequisites

Before starting, I ensured:

* Azure account
* Azure CLI installed → `az login`
* Terraform installed → `terraform -version`
* Ansible installed → `ansible --version`

---

## 🔑 Generate SSH Key

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/azure_key
```

---

## 🏗️ Step 1: Infrastructure Setup (Terraform)

Navigate to Terraform directory:

```bash
cd terraform
```

Initialize Terraform:

```bash
terraform init
```

Preview execution plan:

```bash
terraform plan
```

Apply configuration:

```bash
terraform apply
```

Type `yes` when prompted.

### ✅ Output

After successful execution, I obtained:

* Public IPs of Virtual Machines
* These IPs are used in the Ansible inventory

---

## 🖥️ Step 2: Configure Ansible Inventory

Edit `inventory.ini`:

```ini
[web]
<VM1_PUBLIC_IP>
<VM2_PUBLIC_IP>

[web:vars]
ansible_user=azureuser
ansible_ssh_private_key_file=~/.ssh/azure_key
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

## ⚡ Step 3: Ansible Multi-Play Playbook

The `site.yaml` contains **3 plays**:

### ▶️ Play 1: Install Nginx

* Update package list
* Install nginx
* Start and enable service

### ▶️ Play 2: Deploy Static Website

* Copy `index.html` to server
* Set correct permissions
* Restart nginx

```yaml
src: files/index.html
dest: /var/www/html/index.html
```

### ▶️ Play 3: Verify Deployment

* Send HTTP request
* Validate response status = `200`

```yaml
url: "http://{{ item }}"
```

---

## 🌐 Step 4: Website Content

Static HTML file location:

```bash
ansible/files/index.html
```

🔗 Source:
[https://github.com/pravinmishraaws/Azure-Static-Website](https://github.com/pravinmishraaws/Azure-Static-Website)

---

## ▶️ Step 5: Run Ansible Playbook

Navigate to Ansible directory:

```bash
cd ansible
```

Execute playbook:

```bash
ansible-playbook -i inventory.ini site.yaml
```

---

## ✅ What Happens Internally

* Connects to all Virtual Machines
* Installs Nginx
* Deploys website
* Restarts Nginx
* Verifies deployment automatically

---

## 🌍 Step 6: Access Website

Open in browser:

```bash
http://<VM_PUBLIC_IP>
```

You should see your deployed webpage 🎉

---

## 🔁 Workflow Summary

```text
Terraform → Creates Azure Infrastructure
        ↓
Ansible → Configures Servers
        ↓
Nginx → Hosts Website
        ↓
Browser → Access Website
```

---

## 💡 Key Concepts Demonstrated

* Infrastructure as Code (IaC)
* Configuration Management
* Multi-play Ansible Playbook
* Idempotency
* Remote Provisioning

---

## 🚨 Troubleshooting

### ❌ SSH Permission Denied

```bash
chmod 400 ~/.ssh/azure_key
```

---

### ❌ Ansible Connection Failed

✔ Check:

* VM is running
* Port 22 is open
* Correct IP address

---

### ❌ Website Not Loading

```bash
sudo systemctl status nginx
```

---

## ⭐ Conclusion

In this project, I successfully:

* Provisioned Azure infrastructure using Terraform
* Configured servers using Ansible
* Deployed a static website using Nginx
* Verified deployment automatically

---

## 👨‍💻 Author

**Vishal Gore**

