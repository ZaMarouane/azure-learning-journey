# Azure Network Security Groups (NSGs) — Theory & Hands-On

## 🎯 Goal
Understand **why NSGs exist**, how they work at a **technical level**, configure them **hands-on**, and know **when and how** to use them correctly in real Azure environments.

---

## 1️⃣ Why Network Security Groups?

### The Problem
- VNets are private, but **not secure by default**.
- Any resource in the same VNet can communicate freely.
- We need **network-level traffic control**.

### Why NSGs Exist
- Control **inbound and outbound traffic**.
- Enforce **least privilege networking**.
- Act as Azure’s **basic network firewall**.

### Real-World Use Cases
- Allow SSH/RDP only from admin IPs.
- Restrict application ports (HTTP/HTTPS).
- Block lateral movement between subnets.
- Enforce tier-based access (Web → App → DB).

---

## 2️⃣ What is an NSG?

### Definition
A **Network Security Group (NSG)** is a set of **security rules** that allow or deny **Layer 4 traffic** based on:
- Source IP
- Destination IP
- Port number
- Protocol (TCP/UDP/Any)

✅ **NSGs operate at OSI Layer 4 (Transport Layer)**  
❌ They do **not** inspect payloads (Layer 7).

---

## 3️⃣ Where Can NSGs Be Applied?

### Supported Associations
- **Subnet level** (recommended for most cases)
- **Network Interface (NIC) level**

### Priority Rule
If both exist:

Subnet NSG + NIC NSG → **Most restrictive rule wins**

---

## 4️⃣ NSG Rules Explained

Each NSG rule consists of:

| Field       | Description |
|------------|-------------|
| Priority   | Lower number = higher priority |
| Source     | IP / CIDR / Service Tag |
| Destination| IP / CIDR |
| Port       | Single or range |
| Protocol   | TCP / UDP / Any |
| Action     | Allow / Deny |

⚠️ **Rules are processed in priority order**

---

## 5️⃣ Default NSG Rules (Important)

### Default Inbound Rules
- Allow VNet traffic
- Allow Azure Load Balancer
- Deny all other inbound traffic

### Default Outbound Rules
- Allow internet access
- Allow VNet traffic
- Deny all others

❗ Default rules **cannot be deleted**, only overridden by higher priority rules.

---

## 6️⃣ Hands-On: Create and Apply an NSG

### Step 1: Create NSG
Portal → Network Security Groups → Create

- Name: `nsg-web`
- Resource Group: existing RG
- Region: same as VNet

---

### Step 2: Create Inbound Rule (Allow SSH)

| Setting        | Value           |
|----------------|----------------|
| Priority       | 100            |
| Source         | Your public IP |
| Source Port    | *              |
| Destination    | Any            |
| Destination Port | 22           |
| Protocol       | TCP            |
| Action         | Allow          |

---

### Step 3: Create Inbound Rule (Allow HTTP)

| Setting        | Value           |
|----------------|----------------|
| Priority       | 200            |
| Source         | Any            |
| Destination Port | 80            |
| Protocol       | TCP            |
| Action         | Allow          |

---

### Step 4: Associate NSG to Subnet
- Go to Subnets
- Attach `nsg-web` to `subnet-web`

⚠️ **Best Practice:** Apply NSGs at **subnet level**, not NIC level.

---

## 7️⃣ NSGs in Hub-and-Spoke Architecture

### Recommended Pattern
- **Subnet-level NSGs**
- Central firewall in hub
- NSGs used for **micro-segmentation**

### Example
- Web Subnet → Allow 80/443
- App Subnet → Allow from Web only
- DB Subnet → Allow from App only

---

## 8️⃣ When to Use NSGs

### ✅ Use When
- Basic traffic filtering is required
- You want subnet-level security
- Enforcing east-west traffic rules
- Controlling VM access

### ❌ Do NOT Use When
- Deep packet inspection is needed
- Application-layer filtering required
- Centralized security logging required

➡️ Use **Azure Firewall or NVA**

---

## 9️⃣ Trade-Offs & Comparison

### ✅ Pros
- Free
- Simple and fast
- Native Azure service
- Highly available

### ❌ Cons
- Layer 4 only
- No TLS inspection
- Limited logging
- No threat intelligence

---

## 🔟 Common Mistakes

- Forgetting rule priority order
- Allowing `Any → Any`
- Blocking traffic accidentally with Deny rules
- Mixing NIC and subnet NSGs without planning
- Assuming NSGs replace firewalls

---

## 1️⃣1️⃣ Troubleshooting Checklist

- Check effective security rules
- Verify rule priority
- Confirm correct subnet/NIC association
- Validate source IP ranges
- Test with `ping`, `curl`, `telnet`

---

## 1️⃣2️⃣ Summary

- NSGs operate at **Layer 4 (IP + Ports)**
- Used for **basic network security**
- Best applied at **subnet level**
- Critical for **Azure networking hygiene**
- Complement firewalls, do not replace them

