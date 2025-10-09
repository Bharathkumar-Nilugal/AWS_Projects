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
```
## 🛡️ Backup & Restore

### 9. ✅ Create AMI Backup
To ensure recoverability of your EC2 instance:

- Navigate to **EC2 Console** → Select your instance → **Actions** → **Create Image**
- Provide:
  - **Image Name**: `tdk-ami-image-backup`
  - **Description**: Backup of Apache-configured EC2 instance
- Confirm image creation under the **AMIs** tab. The AMI state should be `Available`.

### 10. 🗑️ Terminate EC2
Once the AMI is successfully created:

- Go to **EC2 Instances**
- Select the original instance
- Click **Instance State** → **Terminate**

### 11. 🔁 Restore EC2 from AMI
To restore the EC2 instance:

- Go to **AMIs** → Select `tdk-ami-image-backup` → **Launch Instance**
- Configuration:
  - **VPC**: Use the same VPC
  - **Subnet**: Use the same private subnet (e.g., `10.0.3.0/24`)
  - **IAM Role**: `Ec2-SSM-Role`
  - **Security Group**: Same SG with HTTP and ICMP rules
- After launch:
  - Use **Session Manager** to connect
  - Run the following to verify Apache:
    ```bash
    sudo systemctl status httpd
    ```

---

## 📊 Monitoring Setup

### 12. 📈 Create CloudWatch Alarm for CPU Usage
To monitor CPU utilization:

- Go to **CloudWatch Console** → **Alarms** → **Create Alarm**
- Select metric:
  - **Namespace**: `AWS/EC2`
  - **Metric Name**: `CPUUtilization`
  - **Dimension**: `InstanceId` → Select your restored EC2
- Set conditions:
  - **Threshold type**: Static
  - **Condition**: Greater than or equal to `80`
  - **Period**: `5 minutes`
  - **Evaluation Periods**: `1`
- Alarm Name: `High-CPU-Alarm`

### 13. 📬 Configure SNS Notification
To receive alerts:

- Go to **SNS Console** → **Create Topic**
  - **Name**: `High-CPU-SNS`
  - **Type**: Standard
- Create **Subscription**:
  - **Protocol**: `Email`
  - **Endpoint**: Your email address
  - Confirm subscription via email link
- Attach SNS topic to alarm:
  - In CloudWatch alarm setup → **Add notification** → Select `High-CPU-SNS`

### 14. 🧪 Verify Alarm
- Go to **CloudWatch → Alarms**
- Confirm alarm status is `OK`
- Simulate high CPU load:

 ```bash
  yes > /dev/null &
```
Wait for alarm to trigger

Check email for SNS alert
