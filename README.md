# MST300 Project 1: Active Directory Domain Services on Azure  
**Seneca College – MST300 | Winter 2025**  
**Status:** Completed ✔️  
**Technologies:** Azure • Windows Server 2019 • Active Directory • DNS • IIS • Subnetting • VNet Peering

---

## 📘 Overview

This project demonstrates how to design and deploy a full **Active Directory Domain Services (AD DS)** environment in **Microsoft Azure**, following the official requirements for **MST300 Project 1** at Seneca College.

The environment includes:

- A **Domain Controller** hosting AD DS + DNS  
- A **Web Server** hosting a custom IIS page  
- A **Client VM** joined to the domain  
- **Three Virtual Networks** with proper peering  
- **Subnetting a /24 network into four /26 subnets**  
- **Azure Bastion** for secure remote access  

This guide is structured for **students completing MST300**, but written with **professional formatting** to make the repository searchable and portfolio-ready.

---

## ⭐ Learning Outcomes

By completing this project, you successfully demonstrated:

- Deployment of **Windows Server 2019** and **Windows 10** in Azure  
- Installation and configuration of **Active Directory**  
- Domain user/group creation and authentication  
- Joining machines to the domain  
- Proper **DNS configuration** inside Azure  
- **Virtual network peering** between three VNets  
- Deployment of an IIS website with a **custom page title**  
- Proper IP addressing through **/26 subnetting**  
- Secure access using **Azure Bastion**  

---

## 🧩 Architecture Summary

This is the required MST300 topology:

MST300-Project1-RG
│
├── VNet1 (131.131.131.0/24)
│ ├── Subnet1 DC (131.131.131.0/26) → Domain Controller
│ └── AzureBastionSubnet (131.131.131.64/26) → Bastion Host
│
├── VNet2 (131.131.131.128/26)
│ └── WebServer Subnet → IIS Web Server
│
└── VNet3 (131.131.131.192/26)
└── Client Subnet → Windows 10 Client


### Virtual Machines

| VM Name | OS | Role | vCPU | RAM | Network |
|--------|-----|------|------|-----|---------|
| **dc-vm** | Windows Server 2019 | Domain Controller + DNS | 2 | 4GB | VNet1 |
| **webserver-vm** | Windows Server 2019 | IIS Web Server | 1 | 2GB | VNet2 |
| **client-vm** | Windows 10 Pro | Domain Client | 1 | 2GB | VNet3 |

---

## 🧮 Subnetting Breakdown (/26)

Your assigned **/24** address space is divided into **four equal /26** subnets:

| Subnet | IP Range | CIDR | Purpose |
|--------|----------|------|----------|
| Subnet 1 | `.0 – .63` | /26 | Domain Controller |
| Subnet 2 | `.64 – .127` | /26 | Bastion |
| Subnet 3 | `.128 – .191` | /26 | Web Server |
| Subnet 4 | `.192 – .255` | /26 | Client VM |

Each /26 provides:
- **64 IP addresses**  
- **62 usable hosts**  
- **Subnet mask:** 255.255.255.192  

---

## 🚀 Deployment Steps (High-Level)

### 1️⃣ Azure Resource Setup
- Create a **resource group**
- Deploy **three VNets** with correct /26 subnets  
- Peer all VNets correctly  
- Deploy Azure Bastion in Subnet2  

### 2️⃣ Domain Controller Setup
- Deploy Windows Server 2019 in VNet1 / Subnet1  
- Set static private IP  
- Install Active Directory Domain Services  
- Create the domain:  studentID.mst300.com

  
- Create:
- Domain admin account  
- Standard user account  

### 3️⃣ DNS Configuration
- Ensure DNS on all VNets points to DC’s private IP  
- Validate with: nslookup studentID.mst300.com


### 4️⃣ Web Server Setup
- Deploy Windows Server 2019 VM in VNet2  
- Join to the domain  
- Install IIS  
- Replace the default IIS page with a **custom title**  

### 5️⃣ Client VM Setup
- Deploy Windows 10 VM in VNet3  
- Configure DNS to DC  
- Join to the domain  
- Test login using domain user  

### 6️⃣ Final Verification (Required for Full Marks)
- Login to **Bastion** using the **domain user** (NOT admin)  
- Access IIS page from client via **FQDN**  
- Show VNet peering  
- Show AD Users and Computers  
- Show all three VMs joined to domain  

---

## 🛠️ Troubleshooting Guide

Common MST300 issues & fixes:

### 🟥 Issue: “Cannot join domain”
✔️ Set DNS on VM to *only* the DC’s private IP  
✔️ Restart VM after DNS change  
✔️ Check VNet peering  
✔️ Ensure firewall on DC allows DNS  

---

### 🟥 Issue: “Bastion cannot connect”
✔️ VM must be in a peered network  
✔️ Bastion subnet must be named EXACTLY:  AzureBastionSubnet

✔️ Wait 2–3 minutes after deployment  

---

### 🟥 Issue: “IIS webpage not loading”
✔️ Check IIS service status  
✔️ Try browsing `localhost` on the web server  
✔️ Flush cache:  ipconfig /flushdns




---

## 📊 Grading Rubric (Seneca MST300)

| Component | Weight | Requirement |
|----------|--------|-------------|
| **Azure Bastion** | 25% | Login using domain *user* account |
| **Web Server** | 25% | Custom IIS page + accessible via FQDN |
| **VNet Peering** | 25% | Three-way peering, no missing routes |
| **Domain Controller** | 25% | Users, domain join, DNS |

---

## 📁 Repository Structure

/
├── README.md ← This file
├── images/ ← Architecture & screenshots
└── resources/ ← Optional: scripts, configs


---

## 🔍 SEO Tags (for discoverability)

**Keywords:**  
Seneca • MST300 • Project 1 • Active Directory • Azure ADDS • VNet Peering • Subnetting /26 • IIS • Windows Server • Azure Bastion • CSN • Seneca College Labs • Student Guide • Walkthrough • Cloud Computing

---

## ⭐ Support the Repo

If this project helped you or future MST300 students, please ⭐ star the repository!

---

**Created with ❤️ by Ahmed for MST300 students and for future cloud/IT recruiters reviewing my work.**


