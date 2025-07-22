# 🧱 AWS 3-Tier Architecture with Bastion Host (Built Using AWS Console - No Terraform)

## 🎄 Introduction

This project is a hands-on setup of a **secure 3-tier AWS infrastructure**, built **entirely using the AWS Management Console (GUI)** — no Terraform or automation tools were used.

-

##  Why I Built This

I’m transitioning into cloud and DevOps roles from an IT support background. Instead of only learning theory or copying IaC code, I chose to build this architecture manually using the AWS console.  
This helped me understand how networking and access control really work behind the scenes.

--

# AWS Secure Infrastructure with Bastion Host – GUI-Based Project

This project demonstrates how to design and deploy a secure 3-tier architecture in AWS **entirely through the Management Console (GUI)**. The setup includes a **Bastion Host for secure SSH access**, **Web Server in private subnet**, and an **RDS MySQL database** – all protected using **NAT Gateway and custom Security Groups**.

✅ **Region:** `us-east-2 (Ohio)`  
✅ **Tools Used:** AWS Console (GUI) only – No Terraform  
✅ **Project Type:** Hands-on Lab with Step-by-Step Screenshots  
✅ **Project Folder:** `C:\Users\DELL\Desktop\Bastion tier 3 project`

---

## 📌 Project Architecture Overview

- A **custom VPC** with public and private subnets across two Availability Zones.
- **Bastion EC2** in a public subnet for secure SSH into private resources.
- **Web EC2** in a private subnet, accessible only via the Bastion.
- **RDS MySQL** in a private subnet, accessible only from the Web EC2.
- **NAT Gateway** for outbound internet access from private subnets.

---

## 🔧 Steps to Deploy

### 1️⃣ Create Custom VPC
- CIDR block: `10.0.0.0/16`  
- Name: `My-Custom-vpc`  
 ![Custom vpc ](./Screenshot/VPC-Dashboard.png)

---

### 2️⃣ Create 4 Subnets

| Subnet Name     | Type    | AZ           | CIDR          |
|-----------------|---------|--------------|---------------|
| PublicSubnet1   | Public  | us-east-2a   | 10.0.1.0/24   |
| PublicSubnet2   | Public  | us-east-2b   | 10.0.2.0/24   |
| PrivateSubnet1  | Private | us-east-2a   | 10.0.3.0/24   |
| PrivateSubnet2  | Private | us-east-2b   | 10.0.4.0/24   |

![Public Subnet1  ](./Screenshot/create-Publicsubnet1.png)
![Public Subnet2  ](./Screenshot/Create-PublicSubnet2.png)
![Private Subnet1 ](./Screenshot/Create-PrivateSubnet1.png)

![Private Subnet2 ](./Screenshot/Create-privateSubnet2.png)

![Subnet  Dashboard ](./Screenshot/Subnet-Dashboard.png)
---

### 3️⃣ Create and Attach Internet Gateway
- Name: `My-IGW`  
- Attach to VPC: `My-Custom-vpc`  
📸 ![IGW ](./Screenshot/Create-InternetGateway.png)

 ![IGW attach to vpc ](./Screenshot/IGW-attach-CustomVPC.png)

![IGW  Dashboard ](./Screenshot/IGW-Dashboard.png)


---

### 4️⃣ Create Public Route Table
- Name: `Public-RT`
- Add route: `0.0.0.0/0 → My-IGW`
- Associate with: `PublicSubnet1` and `PublicSubnet2`  

📸 ![Public RT create ](./Screenshot/Public-RT.png)

##  Public Subnet associate with public route Table 

  ![Public RT P-sub association ](./Screenshot/PublicSubnet1-2-PublicRT.png)


---

### 5️⃣ Create Private Route Table
- Name: `Private-RT`
- Associate with: `PrivateSubnet1` and `PrivateSubnet2`  
📸  [Pcreate Private RT ](./Screenshot/Create-PrivateRT.png)

## Private subnets associate with privte RT

 ![Subnet associatin ](./Screenshot/PrivateSubnet1-2-with-RT.png)
---

### 6️⃣ Allocate Elastic IP
- Name: `My-EIP`  
📸  ![Allocate EIP ](./Screenshot/Allocate-Elastic-IP.png)

---

### 7️⃣ Create NAT Gateway
- Name: `MyNATGW`
- Attach to: `PublicSubnet1`
- Elastic IP: `My-EIP`  
📸 ![create nat & associate EIP  ](./Screenshot/NAT-gateway.png)

![NAT Dashbard](./Screenshot/NATGW-Dashboard.png)


---

### 8️⃣ Update Private Route Table
- Route: `0.0.0.0/0 → MyNATGW` in `Private-RT` 

 ![create nat & associate EIP  ](./Screenshot/Add-NAT-private-RT.png)

---

### 9️⃣ Create Security Groups

**a. Bastion-SG**
- Inbound: `SSH (22)` from `My IP`  
- Outbound: All traffic  
📸  ![Bastion SG ](./Screenshot/Bastion-SG.png)

**b. Web-SG**
- Inbound:
  - `HTTP (80)` from `0.0.0.0/0`
  - `SSH (22)` from `Bastion-SG` (via SG ID)
- Outbound: All traffic  
📸 ![Web-SG ](./Screenshot/Web-SG.png)


**c. RDS-SG**
- Inbound: `MySQL (3306)` from `Web-SG` (via SG ID)
- Outbound: All traffic  
📸  ![RDS-SG ](./Screenshot/RDS-SG.png)

** Security Group Dashboard 
 ![Security-SG ](./Screenshot/Security-Group-Dashboard.png)

---

### 🔟 Launch EC2 Instances

**a. Bastion Host**
- Name: `Bastion-Host`
- Subnet: `PublicSubnet1`
- SG: `Bastion-SG`
- Auto-assign public IP: ✅ Yes  
📸 ![BAstion EC2 ](./Screenshot/Bastion-EC2.png)


**b. Web Server**
- Name: `EC2-Web-Server`
- Subnet: `PrivateSubnet1`
- SG: `Web-SG`
- Auto-assign public IP: ❌ No  
📸  ![BAstion EC2 ](./Screenshot/Web-Server.png)

---

### 1️⃣1️⃣ Create RDS Subnet Group
- Name: `MyRDSSubnetGroup`
- Include: `PrivateSubnet1` and `PrivateSubnet2`  
📸 ![DB subnet creation  ](./Screenshot/Create-DB-SubnetGroup.png)

![DB Subnet DASH ](./Screenshot/DBSubnetGroup-dashboard.png)

---

### 1️⃣2️⃣ Launch RDS MySQL Database
- Engine: MySQL
- DB Name: `My-Database`
- Username/password: _set manually_
- Subnet Group: `MyRDSSubnetGroup`
- VPC: `My-Custom-vpc`
- Public access: ❌ No
- Security Group: `RDS-SG`  

📸 ![BAstion EC2 ](./Screenshot/Creation-step-DB.png)

![BAstion EC2 ](./Screenshot/Creation-DB-steps.png)




---

## ✅ Project Completed.
