![Alt test](/Host_a_Static_Website_on_AWS.png)
---

# 🌐 Host a Static Website on AWS

## 📖 Project Overview

This project demonstrates how to **host a static HTML website on Amazon Web Services (AWS)** using a highly available, fault-tolerant, and secure infrastructure.
The architecture leverages core AWS services such as **VPC**, **EC2**, **Load Balancer**, **Auto Scaling**, **Route 53**, **NAT Gateway**, and more.

All reference diagrams and deployment scripts used for this project are available in this repository.

---

## 🏗️ Architecture Summary

The architecture is designed for **high availability, scalability, and security**. Below is a breakdown of the components used:

1. **Virtual Private Cloud (VPC)**

   * Configured with **public and private subnets** distributed across **two Availability Zones** for fault tolerance.

2. **Internet Gateway (IGW)**

   * Enables communication between instances within the VPC and the Internet.

3. **Security Groups**

   * Serve as virtual firewalls to control inbound and outbound traffic for EC2 instances and load balancers.

4. **Availability Zones (AZs)**

   * Two AZs are used to ensure **redundancy** and **fault tolerance**.

5. **Public Subnets**

   * Host the **NAT Gateway** and **Application Load Balancer (ALB)** to route traffic and manage outbound Internet access.

6. **Private Subnets**

   * Contain **EC2 instances (web servers)** to enhance application security.

7. **EC2 Instance Connect Endpoint**

   * Provides secure administrative access to instances in both public and private subnets.

8. **NAT Gateway**

   * Allows instances in private subnets to access the Internet (e.g., for software updates).

9. **EC2 Instances**

   * Serve the static website content via the **Apache HTTP Server**.

10. **Application Load Balancer (ALB)**

    * Distributes incoming web traffic evenly across multiple EC2 instances in different AZs.

11. **Auto Scaling Group (ASG)**

    * Automatically adjusts the number of running EC2 instances to maintain application performance and availability.

12. **GitHub Integration**

    * Hosts the project’s web files and deployment scripts for **version control** and **collaboration**.

13. **AWS Certificate Manager (ACM)**

    * Manages SSL/TLS certificates to secure web traffic (HTTPS).

14. **Simple Notification Service (SNS)**

    * Sends alerts related to Auto Scaling activities (e.g., instance launch or termination).

15. **Route 53**

    * Manages the domain name and DNS records for the hosted website.

---

## ⚙️ Deployment Script

Below is the bash script used to set up and deploy the static website on an EC2 instance.

```bash
#!/bin/bash
# Switch to the root user to gain administrative privileges
sudo su

# Update all installed packages
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change to the Apache web root directory
cd /var/www/html

# Install Git
yum install git -y

# Clone the project GitHub repository
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git

# Copy all files (including hidden ones) to the web root
cp -R host-a-static-website-on-aws/. /var/www/html/

# Clean up unnecessary files
rm -rf host-a-static-website-on-aws

# Enable Apache to start on system boot
systemctl enable httpd

# Start the Apache service
systemctl start httpd
```

---

## 🚀 How to Deploy

1. **Launch EC2 Instances** in the private subnets created within your custom VPC.
2. **SSH** into the instance using **EC2 Instance Connect Endpoint** or your key pair.
3. **Run the user data script** (above) during EC2 instance launch or manually after connecting.
4. **Access your website** using the DNS name of the **Application Load Balancer** or **Route 53** domain.

---

## 🛠️ Technologies Used

* **AWS VPC, Subnets, NAT Gateway, Internet Gateway**
* **EC2, Auto Scaling Group, Load Balancer**
* **Route 53, SNS, Certificate Manager**
* **Apache HTTP Server**
* **GitHub (Version Control)**

---

## 📁 Repository Contents

* `architecture-diagram.png` – Reference architecture diagram
* `deploy-script.sh` – EC2 deployment script
* `index.html` – Static website homepage
* `README.md` – Project documentation

---

## 🔐 Security Considerations

* EC2 instances are placed in **private subnets** to prevent direct Internet access.
* **NAT Gateway** handles outbound Internet traffic securely.
* **SSL/TLS** certificates encrypt data in transit.
* **Security Groups** enforce strict inbound and outbound traffic rules.

---

## 📢 Notifications

AWS **SNS** is configured to send alerts regarding:

* Auto Scaling activities (launch or terminate)
* Health checks and instance status

---

## 🌍 Domain and DNS

The website’s domain is registered and managed via **Route 53**, which provides:

* DNS record configuration
* Domain name resolution
* Integration with the Application Load Balancer

---

## 🧾 License

This project is open-source and available under the [MIT License](LICENSE).

---



