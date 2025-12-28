# 🧪 Azure Foundations — Hands-On (Azure Portal & Azure CLI)

This document contains **all hands-on exercises** for Azure Foundations.
All tasks are performed using **Azure Portal** and **Azure CLI**.

The focus is on:
- Doing real admin tasks
- Understanding scope and governance
- Building operational habits

---

## 🎯 Hands-On Goals

By completing this lab, I have:

- Created Resource Groups using Portal and CLI
- Practiced Azure CLI usage
- Assigned RBAC roles at correct scopes
- Understood inheritance and governance behavior
- Reviewed cost and security posture
- Cleaned up resources (admin discipline)

---

## 🔧 Prerequisites

- Active Azure subscription
- Azure Portal access
- Azure CLI installed

Login to Azure CLI:

```bash
az login

1️⃣ Management Groups
Azure Portal

Go to All services → Management Groups

Click Add Management Group

Provide:

ID: mg-foundations

Display Name: Foundations MG

Click Save

✅ Management Group created

Azure CLI
# Create a management group
az account management-group create \
  --name mg-foundations \
  --display-name "Foundations MG"

# List management groups
az account management-group list --output table


2️⃣ Subscriptions (Observation / Assigning to MG)

Note: You cannot create new Azure subscriptions via CLI in a free account.
Hands-on is limited to assigning existing subscriptions to Management Groups.

Azure Portal

Open Management Group → mg-foundations

Click Add subscription

Select your subscription

Click Save

Azure CLI

# Assign subscription to management group
az account management-group subscription add \
  --name mg-foundations \
  --subscription <YOUR_SUBSCRIPTION_ID>

# Verify subscription assignment
az account management-group show \
  --name mg-foundations \
  --expand


3️⃣ Resource Groups
Azure Portal

Go to Resource Groups → + Create

Select Subscription: <your subscription>

Name: rg-foundations-portal

Region: West Europe

Click Review + Create → Create

✅ Resource Group created

Azure CLI

# Create a resource group
az group create \
  --name rg-foundations-cli \
  --location westeurope

# List resource groups
az group list --output table


4️⃣ RBAC (Assign Roles at Scope)
Azure Portal

Open Resource Group → Access control (IAM)

Click Add → Add role assignment

Role: Reader

Assign access to: User

Select a user

Click Review + assign

✅ Reader role assigned at RG level

Azure CLI

# Assign Reader role at RG scope
az role assignment create \
  --assignee user@example.com \
  --role Reader \
  --scope /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/rg-foundations-cli

# Verify role assignment
az role assignment list \
  --scope /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/rg-foundations-cli \
  --output table


5️⃣ Cleanup
Azure CLI
# Delete resource group
az group delete --name rg-foundations-cli --yes --no-wait


Azure Portal
Delete rg-foundations-portal manually


⚠️ Notes & Best Practices

Management Groups = hierarchy for governance

Subscriptions = billing & isolation boundary

Resource Groups = lifecycle container

Assign RBAC roles at the least privilege scope

Always clean up test resources to avoid costs

✅ Lab Complete

Management Groups: ✅

Subscription assignment: ✅

Resource Groups: ✅

RBAC hands-on: ✅

Azure Portal & CLI: ✅