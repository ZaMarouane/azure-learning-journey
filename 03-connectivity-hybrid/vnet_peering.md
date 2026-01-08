# Azure VNet Peering — Theory & Hands-On

## 🎯 Goal
Understand **why** Azure VNet Peering exists, **how** it works, configure it **hands-on**, and know **when** to use each peering option in real-world architectures.

---

## 1️⃣ Why VNet Peering?

### Problem
- VNets are **isolated by default**
- Private workloads must communicate **securely**
- Avoid exposing services to the public internet

### Why VNet Peering Exists
- Enable **private communication** between VNets
- Use **Azure backbone network**
- No gateways, no VPN overhead
- Low latency, high bandwidth

### Real-World Use Cases
- Hub-and-spoke architecture
- Shared services (DNS, Firewall, Bastion)
- App tier separation (frontend ↔ backend)
- Multi-subscription networking

---

## 2️⃣ What is VNet Peering?

### Definition
**VNet Peering** connects two Azure Virtual Networks so they act as one logical network for traffic flow.

### Key Characteristics
- Private IP communication
- Uses Azure backbone
- No NAT required
- Non-transitive
- Region-specific or Global peering

---

## 3️⃣ Architecture Example

```
VNet-Hub (10.0.0.0/16)
└─ subnet-hub (10.0.1.0/24)

    ↕ VNet Peering

VNet-Spoke (10.1.0.0/16)
└─ subnet-spoke (10.1.1.0/24)
```

---

## 4️⃣ Hands-On: Create VNet Peering

### Step 1: Create VNets
- `vnet-hub` → 10.0.0.0/16
- `vnet-spoke` → 10.1.0.0/16
⚠️ CIDR ranges **must NOT overlap**

---

### Step 2: Create Peering (Hub → Spoke)

Go to:
`vnet-hub` → Peerings → + Add

**This VNet**
- Peering name: `hub-to-spoke`
- Allow VNet access: ✅ Yes
- Allow forwarded traffic: ❌ No (basic setup)
- Gateway transit: ❌ No

**Remote VNet**
- Peering link name: `spoke-to-hub`
- Virtual network: `vnet-spoke`
- Allow VNet access: ✅ Yes

---

### Step 3: Verify Status
Both VNets should show **Connected**.

Optional: Deploy a VM in each VNet and test connectivity:
```bash
ping 10.1.1.x
# If it works → Peering is successful
```

---

## 5️⃣ Advanced Peering Options

### 5.1 Allow gateway or route server in spoke to forward traffic
- Allows spoke-side gateways to route traffic
- Rare scenarios
- Default: Disabled

### 5.2 Use remote gateway (Gateway Transit)
- Spoke uses hub’s VPN / ExpressRoute gateway
- Centralized hybrid connectivity
- Hub-and-spoke enterprise model
- Must be enabled on **both sides**
  - Hub: Allow gateway transit
  - Spoke: Use remote gateway

---

## 6️⃣ Key Takeaways
- VNet Peering enables **private, low-latency connectivity** between VNets
- Simple to set up and uses Azure backbone
- Cannot replace firewalls; still need NSGs for traffic filtering
- Use advanced options only when necessary (gateway transit, forwarded traffic)

---

## 7️⃣ Troubleshooting
- Check if **peering is connected** in portal
- Ensure **non-overlapping IP ranges**
- Test connectivity with ping/telnet
- Verify **NSG rules** allow traffic across VNets

