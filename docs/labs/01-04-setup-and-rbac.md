# 🏗️ Lab 01: Creating an Azure Resource Group

## Objective
Create the foundational Resource Group that will house all Sentinel-related Azure resources.

## Steps

1. Go to the [Azure Portal](https://portal.azure.com)
2. Search for **"Resource Groups"** in the search bar and click it
3. Click **Create**
4. Fill in the details:
   - **Subscription:** Your Azure subscription
   - **Resource Group Name:** e.g., `Sehgal` (or your preferred name)
   - **Region:** Select the region closest to you
5. Click **Next** or **Review + Create**
6. Click **Create**

✅ Your Resource Group is now created.

---

# 🗂️ Lab 02: Creating a Log Analytics Workspace

## Objective
Create the Log Analytics Workspace — the core data store for all logs ingested into Microsoft Sentinel.

## Steps

1. In the Azure Portal, click the search bar and search for **"Log Analytics Workspace"**
2. Click **Create**
3. Fill in the details:
   - **Subscription:** Your subscription
   - **Resource Group:** The one created in Lab 01
   - **Name:** e.g., `Demo Log Analysis`
   - **Region:** Same as your Resource Group
4. Click **Next**, review, then click **Create**
5. Wait for deployment — click **Go to resource** when complete

✅ Your Log Analytics Workspace is ready.

---

# 🛡️ Lab 03: Deploying Microsoft Sentinel

## Objective
Deploy Microsoft Sentinel on top of the Log Analytics Workspace.

## Steps

1. On the Azure Portal home page, search for **"Sentinel"** and click it
2. Click **Create**
3. Select the Log Analytics Workspace created in Lab 02 (e.g., `Demo Log Analysis`)
4. Click **Add**

✅ Sentinel is now deployed!

> 💡 A **free trial is automatically activated**: 31 days at 10 GB/day. Data beyond 10 GB/day will be billed.

---

# 🔑 Lab 04: Azure RBAC for Sentinel

## Objective
Assign Azure RBAC roles to control access to Sentinel resources.

## Sentinel Built-in Roles

| Role | Access Level |
|---|---|
| **Microsoft Sentinel Reader** | Read-only access to incidents, workbooks, analytics |
| **Microsoft Sentinel Responder** | Reader + manage incidents (assign, dismiss) |
| **Microsoft Sentinel Contributor** | Responder + create/edit analytics rules, workbooks, etc. |
| **Microsoft Sentinel Automation Contributor** | Used by automation to run playbooks |

## Steps

1. Go to **Azure Portal** → **Resource Groups**
2. Select your Resource Group (e.g., `Sehgal`)
3. Click **Access Control (IAM)**
4. Click **Add** → **Add role assignment**
5. Search for `sentinel` and select **Sentinel Contributor**
6. Click **Next** → Select or add a member
7. Click **Review + Assign**

✅ Role assignment complete.
