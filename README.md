# Hybrid Enterprise Lab – Phase 1

This project simulates a hybrid enterprise environment with **on-premises infrastructure (VirtualBox)** connected to **Azure-hosted resources**.  
It demonstrates firewall configuration, Active Directory, hybrid networking, and lays the foundation for SIEM integration.

---

## 🚀 Tech Stack / Tools Used
- **VirtualBox** – virtualization for on-prem lab machines
- **pfSense 2.8.1** – firewall & VPN
- **Windows 10 Pro** – on-prem client machine
- **Windows Server 2025** – Active Directory Domain Controller (DC01)
- **Microsoft Azure** – Resource Group, Virtual Network, Subnets, and VNet Gateway

---

## 🗂️ Lab Topology

![Hybrid Lab Topology](screenshots/hybrid_lab_topology.png)

### On-Premises (VirtualBox)
- **pfSense Firewall**
  - WAN: `192.168.1.206`
  - LAN: `192.168.56.1/24`
- **Windows 10 Client**
  - IP: `192.168.56.10/24`
  - Default Gateway: `192.168.56.1`

### Azure Cloud (East US 2)
- **Resource Group:** `HybridLab`
- **Virtual Network:** `172.16.0.0/16`
  - Subnet 1: `snet-eastus2-1 (172.16.0.0/24)`
  - Subnet 2: `GatewaySubnet (172.16.1.0/27)`
- **VM:** `DC01` – Windows Server 2025
  - Private IP: `172.16.0.4`
  - Role: Active Directory Domain Controller
- **Virtual Network Gateway:** provisioned for future site-to-site VPN

---

## 📌 Phase 1 Objectives
- [x] Deploy pfSense firewall in VirtualBox  
- [x] Configure LAN/WAN networking  
- [x] Deploy Windows 10 client and confirm connectivity through pfSense  
- [x] Create Azure Resource Group, Virtual Network, and Subnets  
- [x] Deploy Windows Server 2025 VM (DC01) as AD Domain Controller  
- [ ] Configure site-to-site VPN (pending)  
- [ ] Join Windows 10 client to Active Directory domain (pending)  

---

## 🛠️ Setup Walkthrough

1. **On-Prem Setup**
   - Install pfSense VM in VirtualBox
   - Assign WAN (`192.168.1.206`) and LAN (`192.168.56.1/24`)
   - Connect Windows 10 client to pfSense LAN

2. **Azure Setup**
   - Create Resource Group: `HybridLab`
   - Deploy VNet: `172.16.0.0/16` with two subnets
   - Deploy Windows Server 2025 VM (`DC01`)  
   - Promote DC01 to Active Directory Domain Controller

3. **Connectivity**
   - Validate Windows 10 can reach pfSense gateway
   - Prepare for site-to-site VPN configuration

---

## 📸 Screenshots

### pfSense Console
![pfSense Console](screenshots/pfsense-console.png)

### pfSense WebGUI
![pfSense WebGUI](screenshots/pfsense-webgui.png)

### Windows 10 Client – IP Configuration
![Windows 10 ipconfig](screenshots/win10-ipconfig.png)

### Azure Resource Group
![Azure RG](screenshots/azure-rg.png)

### Azure Virtual Network
![Azure VNet](screenshots/azure-vnet.png)

### Azure Domain Controller Overview
![Azure DC01](screenshots/dc01-overview.png)

---

## 🔮 Phase 2 Roadmap
- Configure and test **site-to-site VPN** between pfSense and Azure  
- Join Windows 10 client to the Active Directory domain  
- Deploy SIEM (Splunk, ELK, or Wazuh) for monitoring/log collection  
- Simulate attack scenarios and document detections  

---

## 📖 Lessons Learned (so far)
- pfSense is a flexible tool for simulating enterpris

