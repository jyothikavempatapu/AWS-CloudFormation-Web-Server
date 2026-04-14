# -AWS-CloudFormation-Web-Server
# ☁️ AWS CloudFormation Web Server

> Infrastructure as Code — EC2 + Nginx + Security Groups on AWS

---

## 📌 Overview

This project provisions a fully configured **Ubuntu web server on AWS** using a CloudFormation template (Infrastructure as Code). Instead of manually clicking through the AWS console, the entire infrastructure is defined in a single YAML file and deployed automatically.

---

## 🛠️ Technologies Used

- **AWS CloudFormation** — Infrastructure as Code (IaC)
- **Amazon EC2** — Ubuntu web server instance (t2.micro)
- **Nginx** — Web server
- **AWS Security Groups** — Firewall rules (HTTP + SSH)
- **Bash / UserData** — Automated server setup on boot

---

## 🏗️ Architecture

```
Internet
    │
    ▼
Security Group (HTTP :80 open, SSH :22 restricted by CIDR)
    │
    ▼
EC2 Instance (Ubuntu + Nginx)
    │
    ▼
Custom Homepage (displays Region + Public DNS)
```

---

## ⚙️ How It Works

1. The CloudFormation template accepts two parameters:
   - **KeyName** — existing EC2 key pair for SSH access
   - **SSHCIDR** — IP range allowed to SSH (default: university network)

2. On stack creation, AWS automatically:
   - Launches a `t2.micro` Ubuntu EC2 instance
   - Installs and starts Nginx via UserData script
   - Creates a custom homepage showing the AWS region and public DNS
   - Attaches a Security Group allowing HTTP from anywhere and SSH from the specified CIDR

3. Stack Outputs provide:
   - **WebURL** — direct link to the hosted page
   - **PublicDNS** — AWS public DNS name
   - **Region** — deployment region

---

## 🚀 Deployment

### Prerequisites
- AWS account with CloudFormation access
- An existing EC2 Key Pair

### Steps
```bash
# Deploy via AWS CLI
aws cloudformation create-stack \
  --stack-name ubuntu-nginx-server \
  --template-body file://ubuntu-nginx.yaml \
  --parameters ParameterKey=KeyName,ParameterValue=<your-key-name> \
               ParameterKey=SSHCIDR,ParameterValue=<your-ip>/32
```

Or deploy via the **AWS Console**:
1. Go to CloudFormation → Create Stack
2. Upload `ubuntu-nginx.yaml`
3. Fill in parameters → Deploy

---

## 📁 Files

| File | Description |
|------|-------------|
| `ubuntu-nginx.yaml` | CloudFormation template — full infrastructure definition |

---

## 🔐 Security Design

- HTTP (port 80) is open to the public (`0.0.0.0/0`) for web access
- SSH (port 22) is restricted to a specific CIDR block — follows least-privilege principles
- No hardcoded credentials — key pair is passed as a parameter

---

## 📚 Key Concepts Demonstrated

- ✅ Infrastructure as Code (IaC)
- ✅ EC2 provisioning and UserData automation
- ✅ Security Group design (least-privilege networking)
- ✅ CloudFormation Parameters, Resources, and Outputs
- ✅ AWS metadata service usage

---

## 👩‍💻 Author

**Jyothika Vempatapu**
M.S. Cybersecurity — University of South Florida
[LinkedIn](https://linkedin.com/in/jyothika--v) • [GitHub](https://github.com/jyothikavempatapu)
