# 📘 Azure Foundations — Core & Governance

This document summarizes the **core Azure governance concepts** learned during Week 1 of my Azure learning journey.

These foundations define **how Azure environments are structured, secured, and controlled** before any workload is deployed.

---

## 🎯 Learning Objectives

By completing this module, I can:

- Explain Azure’s governance hierarchy
- Organize Azure resources correctly
- Apply least-privilege access using RBAC
- Understand cost visibility and control
- Recognize Azure-native security posture tools

---

## 🧭 Azure Resource Hierarchy

Azure uses a **hierarchical structure** to manage resources at scale.

Management Group
└── Subscription
└── Resource Group
└── Resource


### Why This Hierarchy Exists

- Enables centralized governance
- Supports enterprise-scale environments
- Allows permissions and policies to be applied once and inherited

---

## 🗂️ Management Groups

### What They Are
Management Groups are **containers for subscriptions**.

They allow:
- Applying RBAC at scale
- Applying governance rules across many subscriptions
- Structuring environments (Prod / Non-Prod / Sandbox)

### When to Use
- Multiple subscriptions
- Enterprise or growing Azure environments

### Key Notes
- Optional for small environments
- Permissions applied here affect **all child subscriptions**

---

## 📦 Subscriptions

### What a Subscription Is
A subscription is the **main Azure boundary** for:

- Billing
- Access control
- Resource quotas

### Common Usage Patterns
- Separate Prod and Non-Prod
- Separate teams or departments
- Control spending and access

---

## 🧱 Resource Groups

### What a Resource Group Is
A Resource Group is a **logical container** for Azure resources.

### Key Characteristics
- Resources share the same lifecycle
- Deleting a resource group deletes all resources inside it
- Used for organization and management

### Best Practices
- Group related resources only
- Avoid mixing unrelated workloads
- Use clear and consistent naming

---

## 🔧 Resources

### What Resources Are
Resources are the **actual Azure services**, such as:
- Virtual Machines
- Virtual Networks
- Storage Accounts
- Public IPs

### Important Notes
- Resources live inside a resource group
- Access is inherited from higher scopes

---

## 🔐 RBAC — Role-Based Access Control

### Why RBAC Matters
RBAC ensures:
- Secure access
- Least privilege
- Controlled administration

### RBAC Core Components

| Component | Description |
|---------|------------|
| Security Principal | User, Group, or Service Principal |
| Role Definition | Set of permissions |
| Scope | Level where access applies |
| Role Assignment | Binding role to principal |

---

### Common Built-in Roles

- **Owner** – Full access, including access control
- **Contributor** – Manage resources, no access control
- **Reader** – Read-only access

---

### RBAC Scope Levels

RBAC can be applied at:

- Management Group
- Subscription
- Resource Group
- Resource

Permissions **inherit downward** through the hierarchy.

---

### RBAC Best Practices

- Assign roles to **groups**, not individuals
- Avoid using Owner unless required
- Apply least privilege
- Review access regularly

---

## 💰 Cost Management

### Why Cost Management Is Important
- Azure is pay-as-you-go
- Costs grow silently if unmanaged
- Admins are responsible for cost visibility

### Key Concepts
- Cost analysis by subscription and resource group
- Budgets and alerts
- Tagging for cost tracking

---

## 🛡️ Microsoft Defender for Cloud (Overview)

### What It Is
A cloud-native security posture management tool.

### What It Provides
- Security recommendations
- Visibility into misconfigurations
- Compliance insights

### What It Is Not
- Not a firewall
- Not an antivirus replacement

---

## 🚫 Entra ID (Note)

Hands-on Entra ID configuration was **not performed** due to account limitations.

Conceptually:
- Entra ID manages identity
- RBAC relies on Entra ID identities
- Almost all Azure access flows through Entra ID

This topic will be revisited later.

---

## 🧠 Key Takeaways

- Governance comes **before workloads**
- Azure hierarchy enables scale and control
- RBAC enforces security through inheritance
- Cost management is an ongoing responsibility
- Strong foundations prevent future issues

---

## 🎤 Interview-Ready Summary

> “Azure governance is built on a hierarchy of management groups, subscriptions, resource groups, and resources.  
> RBAC enforces least-privilege access at each scope, cost management controls spending, and Defender for Cloud improves security posture.”

---

## ✅ Status

- Hands-on completed: ✅
- Review-ready notes: ✅
- Interview-ready concepts: ✅
