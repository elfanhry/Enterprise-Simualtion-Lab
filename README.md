# 🏢 Enterprise Network Architecture & Security Simulation

![Network Status](https://img.shields.io/badge/Network_Status-Highly_Available-success)
![Security](https://img.shields.io/badge/Security-Hardened-blue)
![Routing](https://img.shields.io/badge/Routing-Multi--Area_OSPF-orange)

## 📜 Project Overview
A comprehensive, multi-layered enterprise network simulation designed and built using **GNS3** and **Cisco IOSv**. This repository documents the step-by-step evolution of a corporate network from foundational connectivity (v1) to a highly available, fully redundant architecture (v2), culminating in a heavily secured, Zero-Trust enterprise environment with Edge internet connectivity (v3).

![The Whole Setup](./enterprise-simulation-lab-v1/lab-images/the-hole-setup.png)

---

## 🚀 Version 1: Foundation & Core Connectivity

### 🎯 Objective
Establish the core routing and switching architecture for a multi-building enterprise, ensuring full reachability between various departments and a centralized Server Farm.

### ⚙️ Key Implementations
* **Routing Architecture:** Implemented **Multi-Area OSPF** (Area 0 for Backbone, Areas 1 & 2 for Access Buildings, Area 3 for the Server Farm) to ensure scalable and efficient routing.
* **VLAN Segmentation:** Designed a logical topology with dedicated VLANs for different departments.
* **Centralized Services:** Deployed an Alpine Linux server providing centralized **DHCP** services via `dnsmasq`.
* **Routing Verification:**
    * ![Backbone OSPF](./enterprise-simulation-lab-v1/lab-images/backbone-router-interfaces-and-the-routing-table.png)
    * ![Area 1 Core](./enterprise-simulation-lab-v1/lab-images/area1-core-switch-interfaces-and-the-routing-table.png)

---

## 🏗️ Version 2: Full Redundancy, Load Balancing & High Availability

### 🎯 Objective
Transform the network into a fault-tolerant infrastructure by eliminating single points of failure, implementing link aggregation, and optimizing traffic paths using deterministic design principles.

### ⚙️ Architectural Enhancements
* **Backbone Redundancy (Area 0):** Redesigned the backbone with Main and Backup L3 switches, forcing OSPF DR/BDR roles deterministically using Loopback interfaces.
* **Link Aggregation (EtherChannel):** Configured dual EtherChannels (LACP) from every core switch to the backbone, and from the core to the access layer.
    * ![Area 1 EtherChannel Config](./enterprise-simulation-lab-v2.pt1/v2-images/area1-c1-etherchannel-config.png)
* **Deterministic Traffic Engineering:** 
    * **RSTP Load Balancing:** Migrated to Rapid PVST+. Configured Core 1 as the Root Bridge for VLANs 10, 20, 30, and Core 2 as the Root for VLANs 40, 50, 60.
    * ![STP Load Balancing](./enterprise-simulation-lab-v2.pt1/v2-images/area1-c1-stp-loadbalancnig-vlan10-30.png)
    * **Gateway Redundancy (HSRP):** Aligned HSRP Active/Standby roles with STP Roots to ensure optimal routing.
    * ![HSRP Config](./enterprise-simulation-lab-v2.pt1/v2-images/area1-c1-hsrp-config.png)
* **Infrastructure Management:**
    * Expanded centralized DHCP and implemented an automated **TFTP Backup** system.
    * ![DHCP Leases](./enterprise-simulation-lab-v2.p2/v2-images.pt2/DHCP-leases.png)

### 📊 Failover Verification
Continuous ping tests confirmed sub-second convergence during simulated core switch failures.
* ![Failover Test](./enterprise-simulation-lab-v2.pt1/v2-images/area0-main-lyswitch-off-conectivity-test.png)
* ![Inter-Department Connectivity](./enterprise-simulation-lab-v2.p2/v2-images.pt2/conectivity-between-differnt-departments-over-the-network.png)

---

## 🛡️ Version 3: Security, Edge Connectivity & Final Hardening

### 🎯 Objective
Secure the network perimeter and internal Access Layer against common threats, implement granular traffic control based on the Principle of Least Privilege, and establish secure internet connectivity.

### ⚙️ Key Security Enhancements
#### 1. Device Management & RBAC
* **Role-Based Access Control:** Created tiered user accounts (`admin` Privilege 15, `support` Privilege 5) to restrict unauthorized configuration changes.
* **Secure Administration:** Enforced **SSH** for remote management, completely disabling Telnet.
    * ![Users and Privileges](./enterprise-simulation-lab-v3/v3-images/users-and-thier-privilges.png)
    * ![SSH Support Login](./enterprise-simulation-lab-v3/v3-images/ssh-using-support-user.png)

#### 2. Access Layer Hardening (Layer 2 Security)
* **Port Security:** Enabled `sticky` MAC address learning with a strict maximum of 1 device per port.
* **Spanning Tree Protection:** Deployed `Root Guard` on Core-facing interfaces and `BPDU Guard` alongside `PortFast` on all end-user Access ports to neutralize rogue switches.
* **Physical Security:** Administratively shut down all unused ports across the topology.

#### 3. Traffic Control & Zero Trust
* Configured **Extended ACLs** tailored per department. IT maintains full management access, while other departments are strictly limited to necessary internal and internet traffic. Server SSH access is restricted exclusively to the IT subnet.
    * ![Departmental ACLs](./enterprise-simulation-lab-v3/v3-images/acls-sample-HR-IT.png)

#### 4. Perimeter Security & Internet Edge (NAT)
* Configured an Edge Router featuring **NAT Overload (PAT)** for general user internet access and **Static NAT** for the secure Server Farm.
    * ![Edge Router NAT Translation](./enterprise-simulation-lab-v3/v3-images/nat-transilation-on-the-edge-router.png)
    * ![ISP Router NAT Translation](./enterprise-simulation-lab-v3/v3-images/nat-transilation-on-the-isp-router.png)

---

## 📁 Repository Structure
* `/enterprise-simulation-lab-v1`: Contains foundational Multi-Area OSPF configuration, initial DHCP setups, and topology mapping.
* `/enterprise-simulation-lab-v2.pt1` & `/enterprise-simulation-lab-v2.p2`: Contain HSRP, RSTP Load Balancing, and EtherChannel redundancy implementation files.
* `/enterprise-simulation-lab-v3`: Contains Access Layer Hardening, ACLs, NAT configurations, and final security audits.

---
*Architected and Configured as a comprehensive demonstration of Enterprise Network Design, Routing, Switching, and Security Hardening.*
