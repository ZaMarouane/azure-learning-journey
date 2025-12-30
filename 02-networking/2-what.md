# 🌐 Week 2 – Azure Networking (What)

This section covers the **core concepts and components of Azure Networking**.  
Before creating any VNets or subnets, it’s important to understand **what each component is, how it fits together, and its purpose**.

---

## 🎯 Objectives

By the end of this section, you will be able to:

- Understand VNets, subnets, and peering  
- Explain Hub & Spoke architecture  
- Plan IP addresses and subnets  
- Apply NSGs for traffic control  
- Prepare for hands-on deployment

---

## 1️⃣ Core Networking Concepts

| Concept | Description |
|---------|------------|
| **VNet (Virtual Network)** | Isolated network in Azure, like an on-prem LAN |
| **Subnet** | Logical segment within a VNet for resource grouping & security |
| **Hub VNet** | Central network for shared services (VPN, firewall, DNS) |
| **Spoke VNet** | Network for application workloads, connected to hub via peering |
| **VNet Peering** | Connects VNets for private communication |
| **NSG (Network Security Group)** | Firewall rules applied at subnet or NIC level |
| **Address Space (CIDR)** | IP range assigned to a VNet (e.g., 10.0.0.0/16) |
| **Gateway Subnet** | Reserved subnet for VPN Gateway connections |

---

## 2️⃣ Hub & Spoke Architecture

**Visual Diagram:**

         On-Premises
               |
           VPN Gateway
               |
           ┌────────┐
           │  HUB   │
           └────────┘
            /      \
    ┌────────┐    ┌────────┐
    │ SPOKE 1│    │ SPOKE 2│
    └────────┘    └────────┘


**Key Points:**

- Hub contains **shared services**: VPN, firewall, DNS  
- Spokes contain **applications and workloads**  
- Spokes **cannot communicate directly** unless allowed  
- Hub manages **centralized routing and security**  

---

## 3️⃣ IP Address Planning

**Why Important:**

- Prevent overlapping IPs  
- Ensure routing and peering work correctly  
- Plan for future growth (more spokes)

**Example IP Plan:**

| Network | CIDR |
|---------|------|
| Hub VNet | 10.0.0.0/16 |
| Spoke-Dev | 10.1.0.0/16 |
| Spoke-Prod | 10.2.0.0/16 |

**Subnets Example (Hub):**

| Subnet | CIDR | Purpose |
|--------|------|---------|
| GatewaySubnet | 10.0.0.0/27 | VPN Gateway |
| AzureFirewallSubnet | 10.0.0.32/26 | Firewall |
| SharedServices | 10.0.1.0/24 | DNS, monitoring |

---

## 4️⃣ Network Security Groups (NSGs)

- Control inbound/outbound traffic at **subnet or NIC level**  
- Apply **least privilege** principle  

**Example NSG for an App Subnet:**

| Priority | Direction | Source | Destination | Port | Action |
|----------|-----------|--------|------------|------|-------|
| 100 | Inbound | VNet | VNet | Any | Allow |
| 200 | Inbound | Internet | App Subnet | 80,443 | Allow |
| 300 | Inbound | Admin IP | App Subnet | 22/3389 | Allow |
| 4096 | Inbound | Any | Any | Any | Deny |

---

## 5️⃣ Key Takeaways

- VNets = isolated networks, subnets = segmentation  
- Hub & Spoke = standard for enterprise-scale networking  
- Peering allows **secure VNet-to-VNet communication**  
- NSGs enforce traffic rules and **protect resources**  
- IP planning is **essential before deployment**

---

## ✅ Next Step – How

Next, we will **build a Hub & Spoke network in Azure hands-on**:

1. Plan IPs for Hub & Spokes  
2. Create Hub VNet  
3. Create Spoke VNets  
4. Configure VNet Peering  
5. Create NSGs and associate with subnets  
6. Deploy test VMs  
7. Test connectivity
