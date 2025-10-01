# Hybrid Enterprise Lab – Phase 1

This project simulates a hybrid enterprise environment with **on-premises infrastructure (VirtualBox)** connected to **Azure-hosted resources**.  
It demonstrates firewall configuration, Active Directory, hybrid networking, and lays the foundation for SIEM integration.

---

## 🚀 Tech Stack / Tools Used
- **VirtualBox** – virtualization for on-prem lab machines
- **pfSense 2.8.1** – firewall, NAT, WireGuard tunnel
- **Windows 10 Pro** – on-prem client machine
- **Windows Server 2025** – Active Directory Domain Controller (DC01)
- **Ubuntu (Azure)** – WireGuard VPN router
- **Microsoft Azure** – Resource Group, Virtual Network, Subnets, and VNet Gateway

---

## 🗂️ Lab Topology

![Hybrid Lab Topology](screenshots/Hybrid%20Lab%20Topology.png)

### On-Premises (VirtualBox)
- **pfSense Firewall**
  - WAN: `192.168.1.206`
  - LAN: `192.168.56.1/24`
  - WireGuard tunnel: `10.99.99.2/30`
- **Windows 10 Client**
  - IP: `192.168.56.50/24` (DHCP or static tested)
  - Default Gateway: `192.168.56.1`

### Azure Cloud (East US 2)
- **Resource Group:** `HybridLab`
- **Virtual Network:** `172.16.0.0/16`
  - Subnet 1: `snet-eastus2-1 (172.16.0.0/24)`
  - Subnet 2: `GatewaySubnet (172.16.1.0/27)`
- **VM:** `DC01` – Windows Server 2025
  - Private IP: `172.16.0.4`
  - Role: Active Directory Domain Controller
- **VM:** `WireGuardUbuntu`
  - IP: `172.16.0.5`
  - Role: VPN endpoint & forwarder
- **Virtual Network Gateway:** provisioned for future site-to-site VPN

---

## 📌 Phase 1 Objectives
- [x] Deploy pfSense firewall in VirtualBox  
- [x] Configure LAN/WAN networking  
- [x] Deploy Windows 10 client and confirm LAN connectivity  
- [x] Create Azure Resource Group, Virtual Network, and Subnets  
- [x] Deploy Windows Server 2025 VM (DC01) as AD Domain Controller  
- [x] Configure WireGuard tunnel (pfSense ↔ Ubuntu)  
- [ ] Confirm full bidirectional routing between Win10 and DC01 (⚠️ blocked by pfSense stateful issue)  
- [ ] Join Windows 10 client to Active Directory domain (pending)  

---

## 🛠️ Setup Walkthrough

### 1. On-Prem Setup
- Installed pfSense VM in VirtualBox  
- Assigned WAN (`192.168.1.206`) and LAN (`192.168.56.1/24`)  
- Deployed Windows 10 client on LAN  
- Verified Win10 ↔ pfSense LAN connectivity  

### 2. Azure Setup
- Created Resource Group: `HybridLab`  
- Deployed VNet: `172.16.0.0/16` with two subnets  
- Deployed Windows Server 2025 VM (`DC01`)  
- Promoted DC01 to Active Directory Domain Controller  
- Deployed Ubuntu VM as WireGuard VPN router (`172.16.0.5`)  

### 3. WireGuard Tunnel
- Configured tunnel between pfSense (`10.99.99.2`) and Ubuntu (`10.99.99.1`)  
- Added static routes on both sides:  
  - Azure route table: `192.168.56.0/24 → 172.16.0.5`  
  - pfSense route: `172.16.0.0/24 → 10.99.99.1`  

### 4. Connectivity Results
- ✅ pfSense ↔ Azure DC01 ping works both ways  
- ✅ Ubuntu ↔ Win10 ping works both ways  
- ✅ Azure DC01 → Win10 ping succeeds  
- ❌ Win10 → Azure DC01 ping fails (dies after entering tunnel)  

---

## 📸 Screenshots

### pfSense Console
![pfSense Console](screenshots/pfsense-console.png)

### pfSense WebGUI
![pfSense WebGUI](screenshots/pfsense-webgui.png)

### pfSense Routing Table
![pfSense Routing Table](screenshots/pfsense-routing.png)

### pfSense NAT/Firewall Rules
![pfSense NAT Rules](screenshots/pfsense-nat-rules.png)

### Windows 10 Client – IP Configuration
![Windows 10 ipconfig](screenshots/win10-ipconfig.png)

### Windows 10 Ping Failure to DC01
![Win10 ping fail](screenshots/win10-ping-fail.png)

### Azure Resource Group
![Azure RG](screenshots/azure-rg.png)

### Azure Virtual Network
![Azure VNet](screenshots/azure-vnet.png)

### Azure Effective Routes
![Azure Effective Routes](screenshots/azure-effective-routes.png)

### Azure Domain Controller Overview
![Azure DC01](screenshots/dc01-overview.png)

---

## 📖 Lessons Learned
- pfSense handles **traffic it originates** correctly, but drops return traffic for **LAN-to-tunnel sessions** due to stateful inspection.  
- Disabling NAT, floating rules, and reply-to were tested — none resolved the asymmetry.  
- Windows 10 was confirmed to send ICMP into the tunnel, but replies never return.  
- This is a known limitation when using pfSense + WireGuard in nested lab scenarios.  

---

## 🔮 Phase 2 Roadmap
- Acquire dedicated router hardware to properly test site-to-site routing  
- Join Windows 10 client to AD domain once routing issue is resolved  
- Deploy SIEM (Splunk, ELK, or Wazuh) for monitoring/log collection  
- Simulate attack scenarios and document detections  

---

## ⚠️ Current Limitation
This lab reached the limit of what can be achieved with **pfSense running in VirtualBox**.  
Due to stateful routing issues, Win10 → DC01 connectivity cannot be completed.  

✅ Next step: Replace or supplement pfSense with a physical router or different virtual firewall to continue Phase 2.  

