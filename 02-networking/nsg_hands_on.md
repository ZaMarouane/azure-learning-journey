# NSG Hands-On Lab — Three-Tier Azure Application

## 🎯 Lab Objective
Build a **multi-subnet Virtual Network** and secure it using **Azure Network Security Groups (NSGs)** to control **north–south and east–west traffic** at **Layer 4 (IP + Port)** following real-world Azure best practices.

This lab simulates a **production-style three-tier application**:
- Web Tier (Internet-facing)
- App Tier (internal)
- Database Tier (highly restricted)

---

## 🗺️ Lab Architecture

```
Internet
|
v
[ Web Tier ]
VM-Web
Ports: 80 / 443
Subnet: subnet-web (10.10.1.0/24)
|
v
[ App Tier ]
VM-App
Port: 8080
Subnet: subnet-app (10.10.2.0/24)
|
v
[ DB Tier ]
VM-DB
Port: 3306
Subnet: subnet-db (10.10.3.0/24)
```

### Traffic Flow
- Internet → Web (HTTP/HTTPS)
- Web → App (TCP 8080)
- App → DB (TCP 3306)
- ❌ No direct Internet → App/DB access
- ❌ No Web → DB access

---

## 1️⃣ Create the Virtual Network

**Azure Portal → Virtual Networks → Create**

### Basics
- **Name:** `vnet-app`
- **Resource Group:** existing RG
- **Region:** same region for all resources

### IP Addresses
- **Address space:** `10.10.0.0/16`

### Subnets

| Subnet Name | CIDR |
|------------|------|
| subnet-web | 10.10.1.0/24 |
| subnet-app | 10.10.2.0/24 |
| subnet-db  | 10.10.3.0/24 |

✅ Create the VNet

---

## 2️⃣ Create Network Security Groups (NSGs)

Create **one NSG per subnet**.

| NSG Name | Associated Subnet |
|--------|------------------|
| nsg-web | subnet-web |
| nsg-app | subnet-app |
| nsg-db  | subnet-db |

All NSGs:
- Same Resource Group
- Same Region as the VNet

---

## 3️⃣ Configure NSG Rules

### 🔹 NSG-Web (Internet-Facing)

#### Inbound Rules

| Priority | Source | Port | Protocol | Action | Reason |
|--------|--------|------|----------|--------|--------|
| 100 | Any | 80 | TCP | Allow | HTTP access |
| 110 | Any | 443 | TCP | Allow | HTTPS access |
| 120 | Your Public IP | 22 | TCP | Allow | Admin SSH |
| 4096 | Any | Any | Any | Deny | Default deny |

#### Outbound Rules
| Priority | Destination | Port | Protocol | Action | Reason |
|--------|-------------|------|----------|--------|--------|
| 100 | 10.10.2.0/24 | 8080 | TCP | Allow | Web → App |
| 4096 | Any | Any | Any | Allow | Default |

---

### 🔹 NSG-App (Internal Only)

#### Inbound Rules

| Priority | Source | Port | Protocol | Action | Reason |
|--------|--------|------|----------|--------|--------|
| 100 | 10.10.1.0/24 | 8080 | TCP | Allow | Web → App |
| 4096 | Any | Any | Any | Deny | Block all else |

#### Outbound Rules

| Priority | Destination | Port | Protocol | Action | Reason |
|--------|-------------|------|----------|--------|--------|
| 100 | 10.10.3.0/24 | 3306 | TCP | Allow | App → DB |
| 4096 | Any | Any | Any | Allow | Default |

---

### 🔹 NSG-DB (Highly Restricted)

#### Inbound Rules

| Priority | Source | Port | Protocol | Action | Reason |
|--------|--------|------|----------|--------|--------|
| 100 | 10.10.2.0/24 | 3306 | TCP | Allow | App → DB |
| 4096 | Any | Any | Any | Deny | Block all else |

#### Outbound Rules

| Priority | Destination | Port | Protocol | Action | Reason |
|--------|-------------|------|----------|--------|--------|
| 4096 | Any | Any | Any | Allow | Default |

---

## 4️⃣ Associate NSGs to Subnets

Go to each NSG → **Subnets** → Associate

| NSG | Subnet |
|----|--------|
| nsg-web | subnet-web |
| nsg-app | subnet-app |
| nsg-db | subnet-db |

⚠️ **Best Practice:**
Apply NSGs at **subnet level**, not NIC level.

---

## 5️⃣ Tier Responsibilities

### 🌐 Web Tier
- Accepts Internet traffic
- Reverse proxies requests to App Tier
- Tests north–south traffic rules

### ⚙️ App Tier
- Accepts traffic only from Web Tier
- Simulates business logic
- Tests east–west traffic control

### 🗄️ DB Tier
- Accepts traffic only from App Tier
- No Internet exposure
- Tests strict subnet isolation

---

## 6️⃣ Connectivity Tests

1. **Internet → Web**
   - Browser → http://<web-public-ip>
   - Expected: Hello from App Tier ✅
2. **Web → App**
   - `curl http://10.10.2.x:8080` ✅
3. **Internet → App**
   - `curl http://10.10.2.x:8080` ❌ (Blocked by NSG)
4. **Web → DB**
   - `mysql -h 10.10.3.x -P 3306` ❌ (Blocked by NSG)
5. **App → DB**
   - `mysql -h 10.10.3.x -P 3306` ✅

---

## 7️⃣ NSG Validation Summary

| Traffic | Expected |
|---------|----------|
| Internet → Web | ✅ Allowed |
| Internet → App | ❌ Blocked |
| Internet → DB  | ❌ Blocked |
| Web → App       | ✅ Allowed |
| App → DB        | ✅ Allowed |
| Web → DB        | ❌ Blocked |

---

## 8️⃣ Key Takeaways

- NSGs operate at **Layer 4 (Transport)**
- Always test connectivity **tier by tier**
- Subnet-level NSGs are preferred for clarity and manageability
- NSGs complement firewalls but do not replace them

