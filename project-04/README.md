# 🚀 AWS Web Server Deployment - Step-by-Step Guide

This guide walks through setting up a web server on AWS with a secure, scalable network using EC2, VPC, subnets, and Nginx.

---

## 📋 Step-by-Step Implementation

### **Phase 1: Network Infrastructure**
1. **VPC Creation**
2. **Subnet Configuration**
3. **Internet Gateway Setup**
4. **Route Table Configuration**

### **Phase 2: Compute & Security**
5. **EC2 Instance Deployment**
6. **Security Group Configuration**
7. **Key Pair Setup**

### **Phase 3: Application Deployment**
8. **SSH Connection**
9. **Nginx Installation**
10. **Service Verification**

---

## 🔧 Key AWS Services & Concepts

### **Amazon VPC (Virtual Private Cloud)**
A virtual network dedicated to your AWS account.

- **CIDR Block:** Defines IP range (e.g., `10.0.0.0/16`)
- **Subnets:** Smaller IP ranges within the VPC (e.g., `/24`)
- **Route Tables:** Define routing rules
- **Internet Gateway:** Allows internet access to/from the VPC

### **Amazon EC2 (Elastic Compute Cloud)**
Provides scalable compute capacity.

- **Instance Types:** Choose based on workload (e.g., `t2.micro`)
- **AMI:** Template for the OS (e.g., Amazon Linux)
- **Key Pair:** Used for SSH authentication
- **Security Groups:** Control inbound/outbound traffic

---

## 📝 Prerequisites

- Active AWS account
- Basic knowledge of networking & Linux CLI
- SSH client installed (e.g., OpenSSH)
- IAM user with required permissions (EC2, VPC)

---

## 🛠️ Detailed Steps

---

### **Step 1: VPC Configuration**

#### 1.1 Create VPC
- **Name:** Custom (e.g., `my-vpc`)
- **CIDR Block:** `10.0.0.0/16`
- **Tenancy:** Default
- **DNS Hostnames:** Enabled

✅ *Creates an isolated virtual network with room for 65k+ IPs.*

---

#### 1.2 Create Subnets

- **Subnet 1 (Public):**
  - CIDR: `10.0.0.0/24`
  - AZ: Select any (e.g., `eu-north-1a`)

- **Subnet 2 (Public):**
  - CIDR: `10.0.1.0/24`
  - AZ: Another (e.g., `eu-north-1b`)

✅ *Spreads resources across multiple AZs for high availability.*

---

#### 1.3 Create Internet Gateway
- Create an Internet Gateway
- Attach it to your VPC

✅ *Enables internet access for public resources.*

---

#### 1.4 Configure Route Table
- Create a custom Route Table
- Add route: `0.0.0.0/0` → Internet Gateway
- Associate the route table with both public subnets

✅ *Allows traffic to flow from/to the internet.*

---

## 💻 Step 2: EC2 Instance Deployment

### 2.1 Launch an EC2 Instance

- **AMI:** Amazon Linux 2023 (or similar)
- **Instance Type:** `t2.micro` (Free Tier eligible)
- **Key Pair:** Create or use an existing key (download `.pem`)
- **Network:** Select your VPC
- **Subnet:** Choose a public subnet
- **Auto-assign Public IP:** Enable
- **Security Group:** Create a new one or choose existing

✅ *Launches a VM into your network with internet access.*

---

### 2.2 Security Group Configuration

Allow the following inbound rules:

| Type  | Protocol | Port Range | Source         | Description      |
|-------|----------|------------|----------------|------------------|
| SSH   | TCP      | 22         | Your IP only   | Admin access     |
| HTTP  | TCP      | 80         | 0.0.0.0/0      | Web traffic      |

✅ *Controls access to your instance.*

---

## 🌐 Step 3: Application Deployment

### 3.1 Connect to EC2 via SSH

```bash
chmod 400 your-keypair.pem
ssh -i "your-keypair.pem" ec2-user@<your-ec2-public-ip>
```

> Replace `<your-keypair.pem>` and `<your-ec2-public-ip>` with your actual values when connecting or testing.

---

## 🌐 3.2 Install Nginx

```bash
# Update packages
sudo yum update -y

# Install Nginx
sudo yum install nginx -y

# Start the Nginx service
sudo systemctl start nginx

# Enable Nginx to start on boot
sudo systemctl enable nginx
```
## 🧬 Amazon Linux 2023 — Nginx Web Server Setup Guide

### ⚙️ 3.3 Configure Firewall (Optional)

If your distribution includes a firewall (e.g., `firewalld`), enable HTTP access:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

---

### ✅ Verification

1. **Check Nginx status**

   ```bash
   sudo systemctl status nginx
   ```

2. **Verify local access**

   ```bash
   curl http://localhost
   ```

3. **Test in browser**

   ```
   http://<your-ec2-public-ip>
   ```

   **Expected Output:** *Welcome to nginx!*

---

### 🔒 Security Tips

* Restrict SSH access to trusted IPs only
* Use **IAM roles** instead of access keys
* Keep your server patched and updated
* Enable **CloudTrail** for activity logging
* Apply **Network ACLs** for additional security layers

---

### 📊 Best Practices

#### 🎗️ High Availability

* Deploy across **multiple Availability Zones**
* Use **Load Balancers** and **Auto Scaling Groups**

#### 💰 Cost Optimization

* Use **Free Tier** when possible
* Monitor costs via **AWS Cost Explorer**
* Use **Spot Instances** for non-critical/test workloads

#### 📈 Monitoring

* Enable **CloudWatch** for metrics and logs
* Configure **VPC Flow Logs**
* Create **Alarms** for CPU, network, or disk thresholds

---

### 🧬 Troubleshooting

#### ❌ Can’t SSH to EC2?

* Check **Security Group** (Port 22 allowed)
* Verify **key file permissions** (`chmod 400 <key.pem>`)
* Confirm the **instance is running** and reachable

#### 🌐 Web Page Not Loading?

* Confirm **Nginx** is running
* Check **Security Group** (Port 80 allowed)
* Validate **Route Tables** and **Internet Gateway (IGW)** attachment

---

### 🚀 Next Steps

* Add **HTTPS** using **ACM + Load Balancer** or **Certbot**
* Register a **domain** with **Route 53**
* Automate deployment with **CloudFormation** or **Terraform**
* Set up **CI/CD** pipelines for automatic deployments
* Integrate a **database** (e.g., **RDS**, **Aurora**)

---

### 📚 Resources

* [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/)
* [EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)
* [AWS Free Tier](https://aws.amazon.com/free/)
* [AWS Security Best Practices](https://docs.aws.amazon.com/whitepapers/latest/aws-security-best-practices/aws-security-best-practices.pdf)

---

### 💄 License

This guide is for **educational purposes only**.
Use responsibly when deploying to real cloud environments.

---

### 🎉 You're Done!

Your web server is **live**, **secure**, and **publicly accessible**.
Great job building your **cloud-native architecture**! 🚀

---

### ✅ Summary of Updates

* **Removed** sensitive info (key pairs, IPs, subnets)
* **Generalized** setup steps for reuse
* **Optimized Markdown** for GitHub readability
