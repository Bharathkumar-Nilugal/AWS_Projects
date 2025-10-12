# Scalable EC2 Deployment with Application Load Balancer and Nginx

This project demonstrates the deployment of a scalable web application architecture on AWS using EC2 instances managed behind an Application Load Balancer (ALB). The EC2 instances run Nginx to serve custom web pages, showcasing successful load balancing and high availability.

---

## Architecture Overview

- **Virtual Private Cloud (VPC):** Custom VPC with public and private subnets across two Availability Zones.
- **EC2 Instances:** Linux servers in private subnets running Nginx as the web server.
- **Jump Host (Bastion):** EC2 instance in the public subnet to securely SSH into private instances.
- **Application Load Balancer:** Internet-facing ALB distributing HTTP traffic evenly to registered EC2 instances.
- **Security Groups and Routing** configured for secure and reliable communication.

---

## Manual Setup Steps

### 1. Create a VPC and Subnets
- Log in to AWS Console.
- Navigate to **VPC** service.
- Create a new VPC with CIDR block `/16`.
- Create two public subnets in different Availability Zones.
- Create two private subnets in the same Availability Zones.
- Set up route tables:
  - Public route table with Internet Gateway attached.
  - Private route table without direct internet access.

### 2. Launch EC2 Instances
- Launch two Linux EC2 instances in private subnets.
- Open SSH port (22) only from the jump host security group.
- Assign private IPs inside VPC.
- Install and configure Nginx on both instances.
- Customize the default Nginx webpage for instance identification.

### 3. Set Up Jump Host
- Launch an EC2 instance in one of the public subnets.
- Assign public IP to the jump host.
- Configure security group to allow inbound SSH from your IP.
- Use this host to SSH into EC2 instances in private subnets securely.

### 4. Create and Configure an Application Load Balancer
- Navigate to **EC2 Dashboard > Load Balancers**.
- Create an Application Load Balancer, internet-facing, spanning both AZs.
- Configure a listener on port 80 for HTTP.
- Create a Target Group of type `instance` with port 80.
- Register your two private EC2 instances in the target group.
- Set up health check on HTTP port 80.
- Verify both instances show as **healthy**.

### 5. Validate Your Setup
- SSH into the jump host and then into the private EC2 instances to verify Nginx is running (`systemctl status nginx`).
- From jump host, run:
```
- curl -I http://<your_alb_dns_name>
```
to check HTTP response header.
- Open your ALB DNS URL in browser.
- Refresh multiple times and observe which EC2 instance’s custom page is delivered; confirms load balancing working.

---

## Screenshots & Evidence

Screenshots included in the project folder demonstrate:

- VPC, subnets, and route tables setup
- EC2 instances launched and monitored
- Nginx service running status on EC2
- ALB configuration, target group with healthy instances
- Browser showing working load-balanced web pages

---

## Notes

- Use **SSH key pairs** securely for instance access.
- Ensure security groups are tight and only allow necessary traffic.
- Terminate resources when testing is complete to avoid billing.
- Customize Nginx pages or backend apps to suit your needs.

---

## Conclusion

This project exemplifies a best-practice AWS networking and deployment model combining private subnet EC2 instances, secure access via jump hosts, and robust client request balancing via Application Load Balancer — all confirmed through manual validation and monitoring.

---

*This README is part of an EC2 + Load Balancer project demonstrating scalable and secure web application hosting on AWS.*
