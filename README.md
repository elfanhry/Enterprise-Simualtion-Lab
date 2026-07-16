# 🏢 Enterprise Network Architecture & Security Simulation

![Network Status](https://img.shields.io/badge/Network_Status-Highly_Available-success)
![Security](https://img.shields.io/badge/Security-Hardened-blue)
![Routing](https://img.shields.io/badge/Routing-Multi--Area_OSPF-orange)

## 📜 Project Overview
This repository hosts a high-fidelity enterprise network simulation built with **GNS3** and **Cisco IOSv**. The project serves as a technical blueprint for a modern enterprise network, demonstrating the evolution through three critical engineering stages: foundational scalability, high-availability redundancy, and advanced security hardening.

![Enterprise Lab Topology](./Enterprise-Lab-Finale.png)

---

## 🚀 Version 1: Foundation & Core Connectivity
The initial phase focused on building a scalable backbone using structured hierarchical design.

### ⚙️ Core Architecture
* **OSPF Multi-Area Design:** Designed to isolate topology changes and reduce CPU overhead on routers. Area 0 serves as the high-speed backbone, while Areas 1, 2, and 3 house distinct organizational departments and the Server Farm[cite: 1, 6].
* **DHCP & TFTP Management:** Implemented an Alpine Linux server running `dnsmasq` for centralized dynamic IP management across 6 different VLANs. This ensures a clean, manageable IP scheme, while the integrated TFTP server provides a centralized repository for automated device configuration backups[cite: 7].

---

## 🏗️ Version 2: Full Redundancy & Load Balancing
Phase two introduced "Zero Downtime" engineering to ensure business continuity.

### ⚙️ High-Availability & Failover Design
* **Deterministic OSPF Election:** We moved beyond default settings. By manually configuring the `Loopback0` interface priorities, we forced specific Layer 3 switches into DR (Designated Router) and BDR (Backup Designated Router) roles, ensuring predictable traffic patterns across Area 0[cite: 3, 6].
* **Link-Layer Resilience:** Every core switch is dual-homed to the backbone using LACP (EtherChannel) bundles. This design provides both bandwidth aggregation and sub-second physical failover[cite: 1, 5, 14, 15].
* **STP & HSRP Synergy:** 
    * **RSTP Load Balancing:** To maximize hardware utilization, we configured per-VLAN Spanning Tree instances. Core 1 is the Root for VLANs 10-30, while Core 2 is the Root for VLANs 40-60[cite: 5, 7, 14, 15].
    * **Gateway Redundancy (HSRP):** We aligned the HSRP Active/Standby states with the STP Root Bridge roles. This creates an optimized, symmetrical traffic flow where the gateway and the root bridge are always on the same high-priority switch, eliminating inefficient bandwidth usage and ensuring instant failover if a core switch shuts down[cite: 5, 7, 14, 15].

---

## 🛡️ Version 3: Security, Edge Connectivity & Final Hardening
The final phase applies "Defense in Depth" to protect the internal infrastructure.

### ⚙️ Advanced Security Hardening
* **Access Layer Security (Layer 2 Hardening):** 
    * We mitigated rogue switch attacks using `BPDU Guard` and `Root Guard` on all access ports, ensuring the STP topology remains under our absolute control[cite: 1, 2, 8, 9, 10, 13, 14, 15].
    * `Port Security` with sticky MAC learning limits every physical wall jack to exactly one device, rendering unauthorized connections impossible[cite: 1, 2, 8, 9, 10, 13].
* **Zero-Trust Traffic Control:** 
    * We deployed department-specific **Extended ACLs**. By default, internal communication is restricted. The IT management subnet is the only entity granted full administrative SSH access, while departmental communication is strictly regulated to prevent lateral movement of threats[cite: 5, 7, 12, 14, 15].
* **Administrative Hardening:** 
    * Enforced **SSH-only** access to all infrastructure devices, completely disabling Telnet. RBAC was implemented with two distinct privilege levels (Admin 15 / Support 5) to ensure operational security and limit the blast radius of any credential compromise[cite: 3, 6, 11, 12, 14, 15].

### 🌐 Edge & Perimeter Connectivity
* **NAT Architecture:** Configured an Edge Router featuring **NAT Overload (PAT)** for departmental internet access, and **Static NAT** for the Server Farm. This allows internal resources to communicate securely while keeping the internal IP scheme hidden from the public internet[cite: 11].
    * ![NAT Translation Proof](./enterprise-simulation-lab-v3/v3-images/nat-transilation-on-the-edge-router.png)

---

## 📊 Operational Verification Highlights
The design was validated through rigorous testing:
1. **Connectivity Tests:** Verified inter-departmental reachability using standard ICMP and advanced SSH protocols[cite: 8, 9, 11].
2. **Security Audits:** Confirmed NAT translations and ACL filtering via debug and show commands[cite: 11, 15].
3. **Failover Audit:** Confirmed seamless traffic continuity during manual core switch shutdowns using the HSRP/OSPF synchronization[cite: 3, 6].

---
*Architected and Configured as a comprehensive demonstration of Enterprise Network Design, Routing, Switching, and Security Hardening best practices.*
