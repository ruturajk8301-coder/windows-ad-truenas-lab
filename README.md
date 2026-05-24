# 🖥️ Windows Server AD + TrueNAS Hybrid Infrastructure Lab

A modular, enterprise-inspired home lab environment demonstrating centralized identity management and software-defined storage architecture built from scratch inside an isolated virtualization environment using VMware Workstation.

---

# 🏢 Real-World Business Scenario & Problem Solving

## 👥 Scenario
Imagine a medium-sized organization where employees store important company files or documents directly on local laptops without centralized storage, access control, or even recovery mechanisms.

This creates following major risks:
- accidental data loss
- unauthorized access
- inconsistent file management
- difficult user administration

So, this lab was designed to simulate how enterprise environments solve these problems using centralized infrastructure services.

---

## ⚠️ Problems Solved

### 1. Centralized File Storage & Recovery
**Problem:** Employees risk losing important files if local systems fail or files are accidentally deleted.  
**Solution:** Implemented centralized storage using TrueNAS SCALE with OpenZFS datasets and snapshot-based recovery workflows.

### 2. Access Control & Permission Isolation
**Problem:** Not every employee should have access to sensitive folders or internal company data.  
**Solution:** Configured dataset-level ACL permissions to enforce controlled read/write access across SMB network shares.

### 3. Centralized Identity Management
**Problem:** Managing local user accounts individually becomes inefficient, insecure, and operationally frustrating.  
**Solution:** Built a centralized Active Directory environment where authentication and user management are handled from a single Domain Controller.

---

# 🗺️ System Architecture & Network Topology

All virtual machines communicate through an isolated VMware VMnet2 network using the `192.168.2.x/24` subnet framework.

### 🖼️ Project Architecture & Network Diagram
![Network Topology Diagram](PROJECT_SCREENSHOTS/13-Topology-Diagram/01-project-topology-diagram.png.png)

### 📊 Master Portfolio Dashboard
![Project Portfolio Dashboard](PROJECT_SCREENSHOTS/13-Topology-Diagram/00-project-portfolio-dashboard.png.png)

---

## 📌 Infrastructure Nodes

### 🖥️ DC1 — Windows Server 2022 Domain Controller
- Operating System: Windows Server 2022
- IP Address: `192.168.75.129`
- Roles: Active Directory Domain Services (AD DS), DNS Server
- Domain: `rutu.lab`

---

### 💾 TRUENAS — Storage Node
- Operating System: TrueNAS SCALE
- IP Address: `192.168.2.11`
- Storage Backend: OpenZFS
- Services: SMB File Sharing, Snapshot Management, Dataset Isolation

---

### 💻 WINCLIENT — Windows 11 Endpoint
- Operating System: Windows 11
- Dynamic/Static IP Range: Bridges between `192.168.75.x` (Domain operations) and `192.168.2.99` (TrueNAS maintenance alignment)
- DNS Server: `192.168.75.129`
- Domain Joined: `rutu.lab`

---

# 🛠️ Implementation & Technical Milestones

## 📁 1. Storage Infrastructure (TrueNAS SCALE)
- Configured a RAIDZ1 storage pool for single-drive failure tolerance.
- Created structured datasets: `backups`, `media`, `personal`, `vmstore`.
- Configured SMB shares and dataset ACL permissions.

### TrueNAS Verification Screenshots:
* **TrueNAS System Information Overview Dashboard:**
![TrueNAS System Dashboard](PROJECT_SCREENSHOTS/13-Topology-Diagram/02-truenas-system-dashboard.png.png)
* **Storage Dashboard Overview:**
![Pool](PROJECT_SCREENSHOTS/01-Pool/01-pool-health-dashboard.png.png)
* **ZFS Drive Topology Layout:**
![Topology](PROJECT_SCREENSHOTS/01-Pool/02-pool-vdev-topology.png.png)
* **Dataset Tree Structure:**
![Datasets](PROJECT_SCREENSHOTS/02-Datasets/01-dataset-tree-structure.png.png)
* **ACL Permissions Management:**
![ACL](PROJECT_SCREENSHOTS/03-Permissions/01-dataset-acl-editor.png.png)

---

## 🔑 2. Active Directory Infrastructure
- Installed Active Directory Domain Services (AD DS).
- Configured DNS services and forwarders.
- Promoted the server to an authoritative Domain Controller.

### Active Directory Verification Screenshots:
* **Server Manager Dashboard:**
![Server Dashboard](PROJECT_SCREENSHOTS/07-Windows-Server/01-server-manager-dashboard.png.png)
* **AD DS Feature Selection Wizard:**
![AD Feature Select](PROJECT_SCREENSHOTS/08-AD-Installation/01-selecting-ad-dns-roles.png)
* **Domain Controller Promotion Verification:**
![DC Status](PROJECT_SCREENSHOTS/09-Domain-Controller/01-active-directory-domain-status.png.png)
* **Active Directory Users and Computers Console:**
![ADUC Console](PROJECT_SCREENSHOTS/10-AD-Users-Groups/01-aduc-interface.png)
* **Authoritative DNS Zone Config:**
![DNS Records](PROJECT_SCREENSHOTS/11-DNS/01-dns-a-record.png.png)
* **DNS Ping Network Resolution:**
![DNS Ping](PROJECT_SCREENSHOTS/11-DNS/02-dns-ping-resolution.png.png)

---

## 💻 3. Windows Client Domain Join & SMB Integration
- Configured client DNS routing to point directly to the Domain Controller.
- Successfully joined Windows 11 client to the `rutu.lab` domain.
- Mapped SMB network shares directly into Windows File Explorer.

### Client Verification Screenshots:
* **Domain Welcome Success Screen:**
![Domain Welcome](PROJECT_SCREENSHOTS/12-Domain-Join/01-Domain-Login-Success.png)
* **Domain Framework Properties Check:**
![Domain Status Verification](PROJECT_SCREENSHOTS/12-Domain-Join/02-Domain-Login-Success.png)
* **Active Network Drive Mapping:**
![Mapped Drives](PROJECT_SCREENSHOTS/04-Windows-Mapping/01-mapped-network-drives.png)

---

# 🔄 Disaster Recovery Demonstration
A file system recovery workflow was executed and validated using OpenZFS snapshots.

### TrueNAS Recovery Verification Screenshots:
* **Initial Manual Snapshot Capture:**
![Manual Snapshot](PROJECT_SCREENSHOTS/05-Snapshots/manual-snapshot-creation.png.png)
* **Snapshot State Verification Ledger:**
![Snapshot Ledger](PROJECT_SCREENSHOTS/05-Snapshots/snapshot-list-success.png.png)
* **Simulated Disaster File Deletion Phase:**
![Disaster File Deletion](PROJECT_SCREENSHOTS/06-Recovery/01-files-deleted-disaster.png.png)
* **Rollback Target Confirmation Prompt:**
![Rollback Selection](PROJECT_SCREENSHOTS/06-Recovery/02-rollback-warning-confirmation.png.png)
* **Successful Restoration Validation:**
![Data Restored](PROJECT_SCREENSHOTS/06-Recovery/03-file-restored-victory.png)

---

# 🌐 Router Configurations (VyOS Interface Integration)
To bridge communication across separated network subnets safely, VyOS interface routing policies were staged and tested.

### VyOS Performance Screenshots:
* **Cross-Subnet Ping Reachability Success:**
![VyOS Ping](PROJECT_SCREENSHOTS/14-VyOS-Interfaces/01-cross-subnet-ping-success.png.png)
* **Active Routing Interface Tables:**
![VyOS Routes](PROJECT_SCREENSHOTS/14-VyOS-Interfaces/02-vyos-interface-routing.png.png)

---

# 🔧 Troubleshooting & Challenges

### DNS Resolution Failures
- Fixed client DNS misconfiguration causing domain lookup failures by changing settings back to match the DC.

### SMB Authentication Issues
- Cleared stale Windows SMB session conflicts when accounts clashed using:
```cmd
net use * /delete /y
```
