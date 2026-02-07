**Cloud-Network-Access-Control-Architecture**
---
> Secure AWS VPC architecture for an internal application using subnet segmentation, bastion host access, and security groups, designed as part of a Cloud Network & Security internship task.
---

```md
# Cloud Network & Access Control Architecture for Internal Application

This repository documents the design and implementation of a secure cloud network architecture for hosting an internal application on AWS.  
The project focuses on restricting public exposure while allowing controlled employee access using best-practice networking and security principles.

---

## 📌 Project Overview

**Role:** Cloud Network & Security Intern  
**Cloud Provider:** AWS  
**Region:** ap-south-1 (Mumbai)

The architecture ensures that internal applications are protected from external threats while remaining accessible through a secure administrative access path.

---

## 🎯 Objectives

- Design a secure Virtual Private Cloud (VPC)
- Implement subnet-based network segmentation
- Restrict direct internet exposure to internal resources
- Allow access only through defined ports
- Enforce least-privilege access using security groups
- Use basic firewall rules (no IDS/IPS tools)

---

## 🏗️ Architecture Overview

**Pattern Used:**  
Public Subnet + Bastion Host + Private Application Subnet

### Access Flow
```

User Laptop
|
| SSH
v
Bastion Host (Public Subnet)
|
| SSH / Application Port
v
Application Server (Private Subnet)

````

This approach ensures:
- No direct public access to the application server
- A single controlled administrative access path
- Strong network isolation

---

## 🌐 Network Design

### Virtual Private Cloud (VPC)
- CIDR Block: `10.0.0.0/16`
- DNS Resolution & Hostnames: Enabled

### Subnets
| Subnet Name          | CIDR         | Type    | Purpose                          |
|----------------------|--------------|---------|----------------------------------|
| Public-Subnet        | 10.0.1.0/24  | Public  | Bastion Host                     |
| Private-App-Subnet   | 10.0.2.0/24  | Private | Internal Application Server      |

---

## 🚪 Internet Gateway & Routing

### Public Route Table
- `0.0.0.0/0` → Internet Gateway
- Associated with Public-Subnet

### Private Route Table
- Local routing only
- No Internet Gateway route
- Associated with Private-App-Subnet

---

## 🖥️ EC2 Instances

### Bastion Host
- Located in Public-Subnet
- Public IP enabled
- Acts as the only administrative entry point

### Application Server
- Located in Private-App-Subnet
- No public IP assigned
- Accessible only through the Bastion Host

---

## 🔐 Security Groups (Firewall Rules)

### Bastion Host Security Group
- SSH (22): Allowed only from admin public IP

### Application Server Security Group
- SSH (22): Allowed only from Bastion Host Security Group
- HTTP (80): Allowed only from Bastion Host Security Group

This enforces strict access control and defined ports only.

---

## 🔑 Access Method

1. SSH into Bastion Host from local machine:
```bash
ssh -i "key_pair.pem" ec2-user@<Bastion_Public_IP>
````

2. From Bastion Host, access Application Server:

```bash
ssh ec2-user@10.0.2.10
```

---

## ⚠️ Simulated Security Mistakes & Fixes

| Mistake                     | Risk                | Correction                          |
| --------------------------- | ------------------- | ----------------------------------- |
| SSH open to 0.0.0.0/0       | Unauthorized access | Restricted to admin IP & Bastion SG |
| Public IP on App Server     | Direct exposure     | Removed public IP                   |
| IGW route in Private Subnet | Internet exposure   | Used private route table            |

---

## ✅ How This Meets Requirements

* Internal application protected
* No direct public exposure
* Network segmentation enforced
* Only defined ports allowed
* Single administrative access path
* Basic firewall and security group rules only

---

## 📁 Repository Contents

* `/docs` – Project documentation (PDF)
* `/screenshots` – AWS console and SSH access screenshots
* `README.md` – Project overview and architecture explanation

---

## 📌 Notes

This project was completed as part of a hands-on cloud networking and security task to demonstrate foundational AWS networking concepts and best practices.

---

## 👤 Author

**Manishwar S**
Cloud Network & Security Intern



