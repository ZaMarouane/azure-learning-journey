# 🌐 Week 2 – Azure Networking (When & Trade-offs)

This section helps you understand **when to use certain networking patterns and components** in Azure, and the **trade-offs involved** in decisions.

---

## 🎯 Objectives

By the end of this section, you will be able to:

- Decide **when to use Hub & Spoke vs flat VNets**  
- Understand **NSG vs Azure Firewall**  
- Recognize **trade-offs in VNet peering, VPN, and IP planning**  
- Apply **engineering judgment** to real-world scenarios  

---

## 1️⃣ When to Use Hub & Spoke

| Scenario | Recommendation |
|----------|----------------|
| Multiple teams/apps need isolation | Use **Hub & Spoke** |
| Centralized shared services (VPN, firewall, DNS) | Hub is ideal |
| Few apps, small scale | Flat VNet is sufficient |
| Expect growth (new VNets, new regions) | Hub & Spoke scales better |
| Need central routing & security policies | Hub & Spoke simplifies management |

**Key Idea:** Hub & Spoke is **enterprise standard**, flat VNet is simpler but limited.

---

## 2️⃣ When to Use NSG vs Azure Firewall

| Feature | NSG | Azure Firewall |
|---------|-----|----------------|
| Layer | 4 (IP + Ports) | 7 (Application aware) |
| Scope | Subnet / NIC | VNet / Hub / Spoke |
| Use case | Basic traffic filtering, internal isolation | Centralized control, logging, threat detection |
| Cost | Low | Higher |
| Complexity | Simple | Advanced configuration required |

**Key Idea:** Use **NSGs for simple rules**, Azure Firewall for **centralized, enterprise-grade security**.

---

## 3️⃣ Trade-offs in VNet Peering

| Consideration | Trade-Off |
|---------------|-----------|
| Regional Peering | Low latency, low cost |
| Global Peering | Higher cost, possible latency |
| Many-to-many peering | Can get complex with multiple VNets |
| Hub-to-spoke | Easier to manage, scalable, centralized |

> Tip: Avoid **full mesh peering** at large scale; use **Hub & Spoke** for simplicity.

---

## 4️⃣ Trade-offs in VPN vs ExpressRoute

| Feature | VPN | ExpressRoute |
|---------|-----|--------------|
| Connectivity | Over internet | Private connection |
| Latency | Higher | Lower & consistent |
| Cost | Lower | Higher |
| Use case | Dev/test, small workloads | Enterprise, production, high throughput |

---

## 5️⃣ Trade-offs in IP Planning

- **Too small address space** → hard to add new workloads  
- **Too large** → wasted IPs, routing table overhead  
- Plan **subnets for future services** (firewall, bastion, monitoring)  

---

## ✅ Key Takeaways

- Hub & Spoke = scalable, isolated, central management  
- NSGs = layer 4, simple traffic filtering; Azure Firewall = layer 7, advanced security  
- Peering decisions affect cost, latency, and complexity  
- VPN vs ExpressRoute = trade-off between cost, latency, and reliability  
- Proper IP planning avoids headaches later  

---

## 🔜 Next Step

With **Why → What → How → When → Trade-offs** for networking complete, your Week 2 fundamentals are ready.  
Next week, we can move to **Hybrid Connectivity** (VPN, ExpressRoute, DNS integration, private connectivity).

---
