# 🏢 Enterprise Network Architecture & Security Simulation

![Network Status](https://img.shields.io/badge/Network_Status-Highly_Available-success)
![Security](https://img.shields.io/badge/Security-Hardened-blue)
![Routing](https://img.shields.io/badge/Routing-Multi--Area_OSPF-orange)

## 📜 Project Overview
This repository hosts a high-fidelity enterprise network simulation built with **GNS3** and **Cisco IOSv**. The project demonstrates the evolution of a corporate network from foundational connectivity to a highly available, fully redundant architecture, culminating in a heavily secured enterprise environment with Edge internet connectivity.

![Enterprise Lab Topology](./Enterprise-Lab-Finale.png)

---

## 🚀 Version 1: Foundation & Core Connectivity
The initial phase focused on building a scalable backbone using structured hierarchical design.

### ⚙️ Core Architecture
* **OSPF Multi-Area Design:** Designed to isolate topology changes and reduce CPU overhead. Area 0 serves as the high-speed backbone, while Areas 1, 2, and 3 house distinct organizational departments and the Server Farm.
* **Management Services:** Implemented an Alpine Linux server running `dnsmasq` for centralized dynamic IP management, ensuring a clean and manageable IP scheme.

![Backbone OSPF](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v1/lab-images/backbone-router-interfaces-and-the-routing-table.png)

---

## 🏗️ Version 2: Full Redundancy & Load Balancing
Phase two introduced "Zero Downtime" engineering to ensure business continuity.

### ⚙️ High-Availability & Failover Design
* **Deterministic OSPF:** We manually configured `Loopback0` interface priorities to control DR/BDR roles, ensuring predictable traffic patterns.
* **Link-Layer Resilience:** Every core switch is dual-homed to the backbone using LACP (EtherChannel) bundles, providing both bandwidth aggregation and physical redundancy.
* **STP & HSRP Synergy:** We aligned HSRP Active/Standby roles with STP Root Bridge priorities (using Rapid PVST+) to ensure symmetrical traffic flow and minimal latency during failovers.

![EtherChannel Config](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v2.pt1/v2-images/area1-c1-etherchannel-config.png)
![STP Load Balancing](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v2.pt1/v2-images/area1-c1-stp-loadbalancnig-vlan10-30.png)
![DHCP Leases](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v2.p2/v2-images.pt2/DHCP-leases.png)

---

## 🛡️ Version 3: Security, Edge Connectivity & Final Hardening
The final phase applies "Defense in Depth" principles to protect the corporate network.

### ⚙️ Advanced Security Hardening
* **Access Layer Security:** We mitigated rogue switch attacks using `BPDU Guard` and `Root Guard`. `Port Security` with sticky MACs limits every physical port to exactly one device.
* **Zero-Trust Traffic Control:** We deployed department-specific **Extended ACLs**. IT management is restricted to authorized subnets only.
* **Administrative Hardening:** Enforced **SSH-only** access with RBAC (Admin 15 / Support 5 levels) to minimize the blast radius of any credential compromise.

![ACL Configuration](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v3/v3-images/acls-sample-HR-IT.png)
![SSH Support Login](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v3/v3-images/ssh-using-support-user.png)

### 🌐 Edge & Internet Connectivity
* **NAT Architecture:** Configured an Edge Router using **NAT Overload (PAT)** for departmental internet access and **Static NAT** for the Server Farm.

![NAT Translation Proof](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v3/v3-images/nat-transilation-on-the-edge-router.png)
![Debug Verification](https://github.com/elfanhry/Enterprise-Simualtion-Lab/blob/main/enterprise-simulation-lab-v3/v3-images/debug-on-the-isp-router.png)

---
*Architected and Configured as a comprehensive demonstration of Enterprise Network Design, Routing, Switching, and Security Hardening.*
