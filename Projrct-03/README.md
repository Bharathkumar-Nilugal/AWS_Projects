# 🌐 AWS-complete-Secured-infrastructure

This project demonstrates a complete AWS infrastructure setup using VPC, EC2, Systems Manager (SSM), NAT Gateway, Apache Web Server, AMI backup/restore, and CloudWatch monitoring. It showcases secure architecture, automated provisioning, and proactive alerting.

---

## 📁 Project Overview

- **Cloud Provider**: Amazon Web Services (AWS)
- **Region Used**: `us-east-1` (can be customized)
- **Architecture**: 2-tier VPC with public/private subnets
- **Access Method**: AWS Systems Manager (SSM) — no SSH keys
- **Monitoring**: CloudWatch + SNS
- **Backup & Restore**: AMI-based EC2 recovery

---

## 🏗️ Infrastructure Setup

### 1. VPC Creation
- VPC CIDR: `10.0.0.0/16`
- DNS Hostnames: Enabled
- DNS Resolution: Enabled

### 2. Internet Gateway (IGW)
- Created and attached to the VPC

### 3. Subnets
| Type     | Name                        | CIDR         | AZ           |
|----------|-----------------------------|--------------|--------------|
| Public   | Public-Subnet-1             | 10.0.1.0/24  | us-east-1a   |
| Public   | Public-Subnet-2             | 10.0.2.0/24  | us-east-1b   |
| Private  | Private-Subnet-1            | 10.0.3.0/24  | us-east-1a   |
| Private  | Private-Subnet-2            | 10.0.4.0/24  | us-east-1b   |

### 4. Route Tables
- **Public Route Table**:
  - Route: `0.0.0.0/0` → IGW
  - Associated with public subnets
- **Private Route Table**:
  - Route: `0.0.0.0/0` → NAT Gateway
  - Associated with private subnets

### 5. NAT Gateway
- Elastic IP created
- NAT Gateway launched in Public Subnet-1
- Attached to Private Route Table

---

## 🖥️ EC2 Instance Setup

### 6. Launch EC2 in Private Subnet
- AMI: Amazon Linux 2
- Subnet: `Private-Subnet-1`
- IAM Role: `Ec2-SSM-Role`
  - Policies:
    - `AmazonSSMManagedInstanceCore`
    - `AmazonEC2ReadOnlyAccess`
- Security Group:
  - Inbound:
    - HTTP (port 80) — Anywhere
    - ICMP (ping) — Anywhere
  - Outbound: All traffic

### 7. Connect via Systems Manager (SSM)
- Use Session Manager to access EC2 securely

### 8. Install Apache Web Server
```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
