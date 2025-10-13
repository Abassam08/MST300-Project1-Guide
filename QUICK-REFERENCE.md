# MST300 Project 1 - Quick Reference Card

## 📋 Project Checklist

### Phase 1: Planning (10 min)
- [ ] Get assigned /24 network
- [ ] Calculate four /26 subnets
- [ ] Write down all passwords
- [ ] Review complete guide

### Phase 2: Azure Setup (40 min)
- [ ] Create Resource Group: `MST300-project1-rg`
- [ ] Create VNet1 with 2 subnets
- [ ] Create VNet2 with 1 subnet
- [ ] Create VNet3 with 1 subnet
- [ ] Deploy Azure Bastion (wait 15 min)

### Phase 3: Domain Controller (30 min)
- [ ] Create dc-vm (Standard_B2s)
- [ ] Install AD DS role
- [ ] Promote to Domain Controller
- [ ] Create studentID.admin user
- [ ] Create studentID user

### Phase 4: VMs & Peering (30 min)
- [ ] Create webserver-vm
- [ ] Create client-vm
- [ ] Configure DNS on vnet2 & vnet3
- [ ] Create all peering connections (6 total)
- [ ] Join both VMs to domain

### Phase 5: Web Server (20 min)
- [ ] Install IIS on webserver-vm
- [ ] Create DNS A record
- [ ] Modify default page title
- [ ] Test locally

### Phase 6: Permissions (20 min)
- [ ] Configure Group Policy
- [ ] Add Domain Users to RDP group
- [ ] Run gpupdate /force on all VMs
- [ ] Restart all VMs

### Phase 7: Testing (15 min)
- [ ] Login with domain user
- [ ] Access website via FQDN
- [ ] Verify DNS resolution
- [ ] Check AD computers

### Phase 8: Video (10 min)
- [ ] Record demonstration
- [ ] Upload to Microsoft Stream
- [ ] Submit to Blackboard

---

## 🔢 Subnetting Quick Reference

**Given:** x.x.x.0/24

| Subnet | CIDR | First IP | Last IP | Broadcast |
|--------|------|----------|---------|-----------|
| #1 | x.x.x.0/26 | x.x.x.1 | x.x.x.62 | x.x.x.63 |
| #2 | x.x.x.64/26 | x.x.x.65 | x.x.x.126 | x.x.x.127 |
| #3 | x.x.x.128/26 | x.x.x.129 | x.x.x.190 | x.x.x.191 |
| #4 | x.x.x.192/26 | x.x.x.193 | x.x.x.254 | x.x.x.255 |

**Subnet Mask:** 255.255.255.192  
**Hosts per subnet:** 62 usable

---

## 🖥️ VM Configuration

| VM | Size | OS | Network | Purpose |
|----|------|-----|---------|---------|
| dc-vm | B2s | Server 2019 | vnet1 | Domain Controller |
| webserver-vm | B1s | Server 2019 | vnet2 | IIS Web Server |
| client-vm | B1s | Win 10 Pro | vnet3 | Domain Client |

---

## 🔐 Username Formats

### Azure Bastion Login:
✅ `studentID@studentID.MST300.com` (UPN format)  
✅ `.\azureuser` (local admin)  
❌ `DOMAIN\username` (NOT supported)

### Examples:
- Domain Admin: `asabra1.admin@asabra1.MST300.com`
- Domain User: `asabra1@asabra1.MST300.com`
- Local Admin: `.\azureuser`

---

## 💻 Essential PowerShell Commands

```powershell
# Check DNS
ipconfig /all

# Test DNS resolution
nslookup studentID.MST300.com
nslookup webserver-vm.studentID.MST300.com

# Flush DNS
ipconfig /flushdns
ipconfig /registerdns

# Update Group Policy
gpupdate /force

# Test connectivity
Test-NetConnection -ComputerName hostname -Port 80
ping webserver-vm.studentID.MST300.com

# Check domain membership
(Get-WmiObject Win32_ComputerSystem).Domain
```

---

## 🌐 Resource Naming Convention

| Resource Type | Name Format | Example |
|--------------|-------------|---------|
| Resource Group | MST300-project1-rg | MST300-project1-rg |
| Virtual Network | MST300-vnetX | MST300-vnet1 |
| Subnet | vnetX-subnet1 | vnet1-subnet1 |
| Bastion Subnet | AzureBastionSubnet | AzureBastionSubnet |
| VM | role-vm | dc-vm |
| Bastion Host | MST300-BastionHost | MST300-BastionHost |
| Domain | studentID.MST300.com | asabra1.MST300.com |

---

## 🔧 Common Fixes

### Cannot Connect via Bastion
```bash
✓ Check VM is running
✓ Wait 2-3 minutes after restart
✓ Try incognito/private browsing
```

### Cannot Join Domain
```powershell
# Fix DNS
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses ("DC_IP")
ipconfig /flushdns
Restart-Computer
```

### Domain User Cannot Login
```powershell
# On DC, add to Remote Desktop Users
Add-ADGroupMember -Identity "Remote Desktop Users" -Members studentID

# Update Group Policy
gpupdate /force
```

### Website Not Loading
```powershell
# On DC, verify DNS record
nslookup webserver-vm.studentID.MST300.com

# On client, flush DNS
ipconfig /flushdns

# On webserver, test IIS
Test-NetConnection localhost -Port 80
```

---

## 📊 Grading Components (25% each)

1. **Azure Bastion:** Login with domain user (NOT admin)
2. **Web Server:** Access via FQDN, show custom title
3. **Network Peering:** Show 3 VNets properly peered
4. **Domain Controller:** Show domain, users, computers

---

## ⏱️ Video Script Outline (< 7 minutes)

1. **Intro (30s):** Name, CloudLab account, date
2. **Bastion (1m):** Login to client-vm with domain user
3. **Website (1.5m):** Browse to webserver FQDN, show title
4. **Networks (2m):** Show 3 VNets and peering
5. **Domain (2m):** Show AD users and computers
6. **Outro (30s):** Summary

---

## 🚨 Critical Reminders

⚠️ **NEVER use B1s for Domain Controller** - Use B2s (2 vCPU, 4GB RAM)  
⚠️ **ALWAYS restart VMs** after changing VNet DNS settings  
⚠️ **MUST name Bastion subnet** exactly: `AzureBastionSubnet`  
⚠️ **CALCULATE subnets BEFORE** creating VNets  
⚠️ **ADD Domain Users** to Remote Desktop Users group  
⚠️ **USE @ format** for Bastion login: `user@domain.com`  
⚠️ **MODIFY IIS title** to "studentID's Windows Server"  
⚠️ **CREATE DNS A record** for webserver-vm  
⚠️ **RUN gpupdate /force** on all VMs after policy changes  
⚠️ **KEEP video UNDER 7 minutes**

---

## 📞 Get Help

1. Check Troubleshooting section (complete guide)
2. Review PowerShell error messages
3. Verify each step's verification checklist
4. Consult Azure documentation
5. Ask instructor

---

## ✅ Final Verification

Before recording video:

- [ ] Can login to client-vm with studentID@domain.com
- [ ] Website loads at http://webserver-vm.studentID.MST300.com
- [ ] Browser shows custom title
- [ ] All 6 peering connections show "Connected"
- [ ] Both VMs appear in AD Computers
- [ ] Both users appear in AD Users

---

## 📁 Files in This Repository

- **README.md** - Repository introduction
- **MST300-Project1-Complete-Guide.md** - Full walkthrough (48KB)
- **LICENSE** - MIT License
- **GITHUB-UPLOAD-INSTRUCTIONS.md** - How to upload to GitHub
- **QUICK-REFERENCE.md** - This cheat sheet

---

**Print this page for quick reference during your project!**

**Last Updated:** October 13, 2025  
**Version:** 2.0
