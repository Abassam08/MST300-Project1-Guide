# MST300 Project 1: Active Directory Domain Services in Azure

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Azure](https://img.shields.io/badge/Azure-0089D6?logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com/)
[![Windows Server](https://img.shields.io/badge/Windows_Server-2019-0078D6?logo=windows&logoColor=white)](https://www.microsoft.com/windows-server)

## 📋 Project Overview

This repository contains a comprehensive walkthrough guide for implementing a complete Active Directory Domain Services environment in Microsoft Azure. The project demonstrates enterprise-level networking, security, and system administration concepts.

### What You'll Build

- **Domain Controller** (Windows Server 2019) managing authentication and DNS
- **Web Server** (Windows Server 2019) running IIS with custom content
- **Client Workstation** (Windows 10 Pro) for domain access
- **Three Virtual Networks** with proper peering configuration
- **Azure Bastion** for secure remote access
- **Proper IP Subnetting** of a /24 network into four /26 subnets

![Architecture Diagram](https://img.shields.io/badge/Architecture-Three_Tier-blue)

---

## 🎯 Learning Objectives

By completing this project, you will:

- ✅ Deploy and configure Azure Virtual Networks with proper subnetting
- ✅ Implement Azure Bastion for secure VM access
- ✅ Install and configure Active Directory Domain Services
- ✅ Create and manage domain users and groups
- ✅ Join Windows machines to an Active Directory domain
- ✅ Configure DNS services for domain resolution
- ✅ Deploy and customize IIS web server
- ✅ Implement Virtual Network Peering
- ✅ Apply Group Policy for user permissions
- ✅ Troubleshoot common domain and networking issues

---

## 🎯 This Guide is Perfect For:

- 📖 Completing your MST300 Project 1
- 🎥 Recording your video demonstration
- 🔧 Troubleshooting common issues
- 💼 Adding to your professional portfolio
- 🤝 Helping classmates (if allowed)
- 📚 Future reference

---

## 📚 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Architecture](#architecture)
3. [Getting Started](#getting-started)
4. [Project Components](#project-components)
5. [Subnetting Guide](#subnetting-guide)
6. [Troubleshooting](#troubleshooting)
7. [Contributing](#contributing)
8. [License](#license)

---

## 🔧 Prerequisites

### Required Knowledge
- Basic understanding of Windows Server administration
- Familiarity with Active Directory concepts
- Basic networking knowledge (IP addressing, DNS, subnetting)
- Azure Portal navigation

### Required Resources
- Azure for Students account (CloudLab)
- Assigned /24 network address space
- Student ID for naming conventions
- 2-3 hours of dedicated time

### Tools Needed
- Web browser (Chrome, Edge, or Firefox recommended)
- Screen recording software (for project submission)
- Text editor for documentation

---

## 🏗️ Architecture

### Network Topology

```
MST300-project1-rg (Resource Group)
│
├── MST300-vnet1 (131.131.131.0/24)
│   ├── vnet1-subnet1 (131.131.131.0/26) ──────► Domain Controller
│   └── AzureBastionSubnet (131.131.131.64/26) ─► Azure Bastion
│
├── MST300-vnet2 (131.131.131.128/26)
│   └── vnet2-subnet1 ──────────────────────────► Web Server (IIS)
│
└── MST300-vnet3 (131.131.131.192/26)
    └── vnet3-subnet1 ──────────────────────────► Client Workstation
```

### Virtual Machines

| VM Name | OS | vCPU | RAM | Role | Network |
|---------|-----|------|-----|------|---------|
| dc-vm | Windows Server 2019 | 2 | 4GB | Domain Controller + DNS | vnet1 |
| webserver-vm | Windows Server 2019 | 1 | 2GB | IIS Web Server | vnet2 |
| client-vm | Windows 10 Pro | 1 | 2GB | Domain Client | vnet3 |

### Domain Configuration

- **Domain Name:** studentID.MST300.com
- **Domain Admin User:** studentID.admin
- **Standard Domain User:** studentID
- **Domain Functional Level:** Windows Server 2016

---

## 🚀 Getting Started

### Quick Start

1. **Clone this repository:**
   ```bash
   git clone https://github.com/abassam08/MST300-Project1-Guide.git
   cd MST300-Project1-Guide
   ```

2. **Read the complete guide:**
   Open `MST300-Project1-Complete-Guide.md` and follow step-by-step instructions

3. **Plan your subnetting:**
   Use your assigned /24 network and calculate four /26 subnets

4. **Follow the walkthrough:**
   Complete each section in order, verifying as you go

5. **Record your demo:**
   Follow the video recording script (under 7 minutes)

### Estimated Timeline

- **Planning & Subnetting:** 10 minutes
- **Azure Setup:** 30-40 minutes
- **Domain Controller Configuration:** 30 minutes
- **VM Deployment & Domain Join:** 30 minutes
- **IIS & DNS Configuration:** 20 minutes
- **User Permissions:** 20 minutes
- **Testing & Verification:** 15 minutes
- **Video Recording:** 10 minutes

**Total Time:** 2-3 hours

---

## 📦 Project Components

### Phase 1: Foundation
- Resource Group Setup
- Virtual Network Creation (with proper subnetting)
- Azure Bastion Deployment

### Phase 2: Infrastructure
- Domain Controller VM Deployment
- Active Directory Domain Services Installation
- Domain Creation and Promotion

### Phase 3: Domain Services
- Web Server VM Deployment
- Client VM Deployment
- Virtual Network Peering Configuration

### Phase 4: Integration
- DNS Configuration
- Domain Join Operations
- IIS Installation and Configuration

### Phase 5: Access Control
- User Account Creation
- Group Policy Configuration
- Permission Management

### Phase 6: Validation
- Comprehensive Testing
- Troubleshooting
- Video Demonstration

---

## 🔢 Subnetting Guide

### Understanding /26 Subnetting

Your assigned /24 network (256 addresses) will be divided into **four equal /26 subnets** (64 addresses each):

| Subnet | Purpose | Range | CIDR |
|--------|---------|-------|------|
| Subnet 1 | Domain Controller | x.x.x.0 - x.x.x.63 | /26 |
| Subnet 2 | Azure Bastion | x.x.x.64 - x.x.x.127 | /26 |
| Subnet 3 | Web Server | x.x.x.128 - x.x.x.191 | /26 |
| Subnet 4 | Client | x.x.x.192 - x.x.x.255 | /26 |

### Example Calculation

**Given:** 131.131.131.0/24

**Result:**
```
vnet1-subnet1:       131.131.131.0/26
AzureBastionSubnet:  131.131.131.64/26
vnet2-subnet1:       131.131.131.128/26
vnet3-subnet1:       131.131.131.192/26
```

**Subnet Mask:** 255.255.255.192

**Usable Hosts per Subnet:** 62

For detailed subnetting instructions, see the complete guide.

---

## 🔧 Troubleshooting

### Common Issues

#### 1. Cannot Connect via Bastion
- ✔️ Verify VM is running
- ✔️ Check Bastion deployment status
- ✔️ Confirm VM is in peered network
- ✔️ Wait 2-3 minutes after VM restart

#### 2. Domain Join Fails
- ✔️ Verify DNS settings point to DC
- ✔️ Check network peering status
- ✔️ Restart VM after DNS changes
- ✔️ Test DNS resolution: `nslookup domain.com`

#### 3. Domain User Cannot Login
- ✔️ Add user to Remote Desktop Users group
- ✔️ Configure Group Policy for logon rights
- ✔️ Run `gpupdate /force` on all VMs
- ✔️ Use correct username format: `user@domain.com`

#### 4. Website Not Accessible
- ✔️ Verify DNS A record exists
- ✔️ Check IIS is running
- ✔️ Test from webserver locally first
- ✔️ Flush DNS cache: `ipconfig /flushdns`

For complete troubleshooting guide, see the main documentation.

---

## 📊 Grading Rubric

| Component | Weight | Key Requirements |
|-----------|--------|------------------|
| Azure Bastion | 25% | Login with domain user (not admin) |
| Web Server | 25% | Access via FQDN, custom title displayed |
| Virtual Network Peering | 25% | Three VNets properly peered |
| Domain Controller | 25% | Domain, users, and domain-joined computers |

### Important Notes
- Must use CloudLab account
- Must use assigned IP address space
- Must follow naming conventions
- Video must be under 7 minutes
- Must demonstrate all four components

---

## 📖 Documentation Structure

```
MST300-Project1-Guide/
│
├── README.md                                  (This file)
├── MST300-Project1-Complete-Guide.md         (Full walkthrough)
├── images/                                    (Architecture diagrams)
├── scripts/                                   (PowerShell automation)
└── resources/                                 (Additional materials)
```

---

## 🤝 Contributing

This guide is maintained for MST300 students at Seneca College. If you find errors or have suggestions:

1. Fork the repository
2. Create a feature branch (`git checkout -b improvement/your-improvement`)
3. Commit your changes (`git commit -m 'Add helpful tip'`)
4. Push to the branch (`git push origin improvement/your-improvement`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Seneca College** - MST300 Course
- **Microsoft Azure** - Cloud platform
- **Students** - Feedback and issue identification
- **Instructors** - Project requirements and guidance

---

## 📞 Support

If you encounter issues:

1. **Check the Troubleshooting section** in the complete guide
2. **Review error messages** carefully
3. **Use PowerShell commands** for diagnostics
4. **Consult Azure documentation** for service-specific issues
5. **Ask your instructor** for clarification on requirements

---

## 🔗 Useful Links

- [Azure Portal](https://portal.azure.com)
- [Azure Documentation](https://docs.microsoft.com/azure/)
- [Active Directory Documentation](https://docs.microsoft.com/windows-server/identity/ad-ds/)
- [Subnet Calculator](https://www.subnet-calculator.com/)
- [PowerShell Documentation](https://docs.microsoft.com/powershell/)

---

## 📈 Project Status

![Status](https://img.shields.io/badge/Status-Active-success)
![Version](https://img.shields.io/badge/Version-2.0-blue)
![Last Updated](https://img.shields.io/badge/Last_Updated-Oct_2025-orange)

**Current Version:** 2.0 (Complete with Subnetting Guide)  
**Last Updated:** October 13, 2025  
**Course:** MST300 - Winter 2025

---

## ⭐ Star This Repository

If you find this guide helpful, please consider giving it a star! ⭐

---

### ☕ Support the Project
If this guide helped you, consider supporting my work:  
https://buymeacoffee.com/Asabra1

## 🔎 Project Keywords (SEO & Discoverability)

This project is associated with the following topics, helping students and recruiters discover it more easily through GitHub and search engines:

**Seneca College**, **Seneca Polytechnic**, **MST300**,  
**MST300 Project 1**, **MST300 AD DS**,  
**Active Directory Domain Services**, **Azure ADDS**,  
**Azure Networking**, **Domain Controller Setup**,  
**Windows Server 2019**, **DNS Configuration**,  
**Virtual Network Peering**, **Azure Bastion**,  
**Subnetting /24 into /26**, **CSN Program**,  
**Seneca Networking Assignment**,  
**IIS on Windows Server**, **Azure VM Deployment**,  
**Group Policy Management**, **Domain Join Troubleshooting**

These keywords increase visibility for future students searching for help with **“MST300 Project 1 Active Directory on Azure”** and help recruiters understand the technologies involved.

---

## 📘 Additional Notes

This project was completed as part of the *Seneca – Computer Systems Networking (CSN)* program.  
It serves as a hands-on demonstration of:

- Azure infrastructure deployment  
- Active Directory setup and administration  
- Enterprise networking fundamentals  
- DNS & Group Policy configuration  
- Secure remote management using Azure Bastion  

Future students who find this repository can use it as a reference for architecture, subnetting, best practices, and troubleshooting for MST300 Project 1.



---

**Built with ❤️ for MST300 Students**
