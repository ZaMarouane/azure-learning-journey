# 🌐 Week 2 – Azure Networking (How)

This section provides **hands-on steps to create a Hub & Spoke network in Azure**, including VNets, subnets, NSGs, peering, and testing connectivity.

---

## 🎯 Objectives

By the end of this section, you will be able to:

- Build a Hub & Spoke network in Azure  
- Configure VNets, subnets, and peering  
- Apply NSGs for traffic control  
- Deploy test VMs and validate connectivity  

---

## 1️⃣ Step 1: Plan Your IP Addresses

| VNet / Subnet       | CIDR         | Purpose                       |
|--------------------|-------------|--------------------------------|
| Hub VNet           | 10.0.0.0/16 | Central hub for shared services |
| Hub Subnets        | 10.0.0.0/27 | GatewaySubnet (VPN)           |
|                    | 10.0.1.0/24 | SharedServices (DNS, monitoring) |
| Spoke-Dev VNet     | 10.1.0.0/16 | Development workloads          |
| Spoke-Prod VNet    | 10.2.0.0/16 | Production workloads           |

> Tip: Keep a small, separate subnet for future services (Azure Firewall, Bastion).

---

## 2️⃣ Step 2: Create Hub VNet

1. Azure Portal → Virtual Networks → + Create  
2. Name: **Hub-VNet**, Address Space: 10.0.0.0/16  
3. Region: your preferred region  
4. Add subnets:
   - GatewaySubnet → 10.0.0.0/27  
   - SharedServices → 10.0.1.0/24  
5. Review & Create

---

## 3️⃣ Step 3: Create Spoke VNets

**Spoke-Dev:**
- Name: Spoke-Dev
- Address Space: 10.1.0.0/16
- Subnets: AppSubnet → 10.1.0.0/24

**Spoke-Prod:**
- Name: Spoke-Prod
- Address Space: 10.2.0.0/16
- Subnets: AppSubnet → 10.2.0.0/24

> Tip: Keep subnet names consistent for easier management.

---

## 4️⃣ Step 4: Set Up VNet Peering

1. Go to **Hub VNet → Peerings → + Add**  
   - Name: Hub-to-Spoke-Dev  
   - Select **Spoke-Dev** as peer  
   - Enable **Allow forwarded traffic** and **Allow gateway transit**  
2. Repeat for Spoke-Prod  
3. Then create **Spoke → Hub** peering  
   - Enable **Use remote gateways** on Spoke VNets  

> Peering is bidirectional; configure both directions.

---

## 5️⃣ Step 5: Create NSGs

**Network Security Groups (NSGs)** filter traffic **at Layer 4 (transport layer)** using **IP addresses and port numbers**. They do not inspect application-level data (Layer 7).

**Example NSG for AppSubnet (Dev/Prod):**

| Priority | Direction | Source | Destination | Port | Action |
|----------|-----------|--------|------------|------|-------|
| 100      | Inbound   | VNet   | VNet       | Any  | Allow |
| 200      | Inbound   | Internet | AppSubnet | 80,443 | Allow |
| 300      | Inbound   | Admin IP | AppSubnet | 22 (Linux) | Allow |
| 4096     | Inbound   | Any    | Any        | Any  | Deny |

**Steps to create:**
1. Azure Portal → Network Security Groups → + Create  
2. Add rules as above  
3. Associate with **AppSubnet** of each spoke  

---

## 6️⃣ Step 6: Deploy Test VMs

- Deploy 1 VM in **Spoke-Dev AppSubnet**  
- Deploy 1 VM in **Spoke-Prod AppSubnet**  
- Optionally deploy 1 VM in **Hub SharedServices subnet**  

**Check connectivity:**
- VM in Spoke-Dev → Hub VM: ✅ should work  
- VM in Spoke-Dev → Spoke-Prod VM: ✅ should fail (isolated)  

---

## 7️⃣ Step 7: Test Connectivity

Use Azure **Network Watcher** or VM ping:

```bash
# From Spoke-Dev VM
ping <Hub-VM-IP>
ping <Spoke-Prod-VM-IP>

