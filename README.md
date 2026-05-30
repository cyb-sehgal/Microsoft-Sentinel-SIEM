# Microsoft Sentinel SIEM Project



![Microsoft Sentinel](https://img.shields.io/badge/Microsoft%20Sentinel-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-Kusto%20Query%20Language-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

**A complete, hands-on Microsoft Sentinel SIEM deployment and threat hunting project built on Azure.**

[Labs](#-labs-overview) • [Architecture](#-architecture) • [KQL Queries](#-kql-queries) • [Setup Guide](#-getting-started) • [Key Learnings](#-key-learnings)



---

## Project Overview

This project documents an end-to-end implementation of **Microsoft Sentinel**  Microsoft's cloud-native SIEM (Security Information and Event Management) and SOAR (Security Orchestration, Automation, and Response) solution on Azure.

The project covers everything from initial deployment to advanced threat hunting, automation, and integration with the Microsoft Defender ecosystem - making it a comprehensive reference for anyone pursuing a **SOC Analyst**, **Cloud Security**, or **Azure Security Engineer** role.

> **Completed using an Azure Student Subscription.** All steps, notes, and workarounds are documented for reproducibility.

---

##  Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Azure Subscription                       │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    Resource Group                        │   │
│  │                                                          │   │
│  │   ┌────────────────────┐    ┌──────────────────────┐     │   │
│  │   │  Log Analytics     │◄───│  Data Connectors     │     │   │
│  │   │  Workspace         │    │  • Threat Intel TAXII│     │   │
│  │   └────────┬───────────┘    │  • Microsoft Entra ID│     │   │
│  │            │                │  • Windows Security  │     │   │
│  │            ▼                │  • Defender for Cloud│     │   │
│  │   ┌────────────────────┐    └──────────────────────┘     │   │
│  │   │  Microsoft         │                                 │   │
│  │   │  Sentinel          │    ┌──────────────────────┐     │   │
│  │   │                    │◄───│  Virtual Machines    │     │   │
│  │   │  • Analytics Rules │    │  • Windows Server    │     │   │
│  │   │  • Threat Hunting  │    │  • Ubuntu Linux      │     │   │
│  │   │  • UEBA            │    └──────────────────────┘     │   │
│  │   │  • Automation      │                                 │   │
│  │   │  • Workbooks       │    ┌──────────────────────┐     │   │
│  │   │  • Watchlists      │◄───│  Defender Portal     │     │   │
│  │   │  • Notebooks       │    │  • Defender XDR      │     │   │
│  │   └────────────────────┘    │  • Defender for Cloud│     │   │
│  │                             └──────────────────────┘     │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Prerequisites

Before starting this project, make sure you have the following ready:

| Requirement | Details |
|---|---|
| ☁️ Azure Account | Free or Student subscription works |
| 🔑 Active Subscription | Azure Free / Student Subscription |
| 📦 Resource Group | Created during setup |
| 📊 Log Analytics Workspace | Core dependency for Sentinel |
| 🔑 OpenAI API Key | Required for the ChatGPT Playbook lab only |
| 🐧 Kali Linux VM | Required for Brute Force Simulation lab |

---

## Labs Overview

| # | Lab | Category | Status |
|---|-----|----------|--------|
| 01 | Creating Azure Resource Group | Setup |  Done |
| 02 | Creating Log Analytics Workspace | Setup |  Done |
| 03 | Deploying Microsoft Sentinel | Setup |  Done |
| 04 | Azure RBAC for Sentinel | Identity & Access |  Done |
| 05 | Content Hub & Data Connectors | Data Ingestion |  Done |
| 06 | Ingesting Threat Intelligence (TAXII) | Threat Intel |  Done |
| 07 | Ingesting Microsoft Entra ID | Data Ingestion |  Done |
| 08 | Windows Security Events with AMA & DCR | Data Ingestion |  Done |
| 09 | Analytic Rules (All 7 Types) | Detection |  Done |
| 10 | Incident Dashboard | SOC Operations |  Done |
| 11 | Ingestion Delay & Latency Workbook | Monitoring |  Done |
| 12 | Writing KQL Queries | Threat Hunting |  Done |
| 13 | Threat Hunting in Sentinel | Threat Hunting |  Done |
| 14 | Hunt for Entra ID Events | Threat Hunting |  Done |
| 15 | Threat Intelligence in Sentinel | Threat Intel |  Done |
| 16 | UEBA Configuration | Behavioral Analytics |  Done |
| 17 | Automation Rules | SOAR |  Done |
| 18 | Playbook with MITRE ATT&CK & ChatGPT | SOAR |  Done |
| 19 | Creating Workbooks | Visualization |  Done |
| 20 | Watchlists & Analytic Rule Integration | Detection |  Done |
| 21 | Notebooks with MSTICPy | ML & Analysis |  Done |
| 22 | Cost Optimization Workbook | FinOps |  Done |
| 23 | Azure DevOps Integration | IaC & CI/CD |  Done |
| 24 | Defender XDR RBAC Configuration | Identity & Access |  Done |
| 25 | Microsoft Defender for Cloud | Cloud Security |  Done |
| 26 | SSH Brute Force Simulation (Kali) | Attack Simulation |  Done |
| 27 | MITRE ATT&CK Overview in Sentinel | Framework |  Done |

---

## KQL Queries

All custom KQL queries used throughout this project are stored in the [`queries/`](queries/) directory.

### Quick Reference

```kql
// ── Verify Threat Intelligence Indicators ──
ThreatIntelligenceIndicator
| take 100

// ── Detect New Process Creation (last 3 days) ──
SecurityEvent
| where EventID == "4688"
| where TimeGenerated > ago(3d)

// ── Count Security Events ──
SecurityEvent
| where EventID == "4688"
| where TimeGenerated > ago(3d)
| count

// ── Summarize Events by Computer ──
SecurityEvent
| where EventID == "4688"
| where TimeGenerated > ago(3d)
| summarize count() by Computer

// ── Entra ID: New User Added ──
AuditLogs
| where OperationName == "Add user"

// ── Entra ID: User Deleted ──
AuditLogs
| where OperationName == "Delete user"

// ── Watchlist Integration ──
let PentestingIPAddresses = _GetWatchlist('PentestingIPAddresses') | project IPAddress;
// ... rest of your rule query
```

 See the full [KQL Queries Reference](queries/README.md)

---

##  Concepts Covered

| Topic | Description |
|---|---|
| **SIEM** | Security Information and Event Management |
| **SOAR** | Security Orchestration, Automation, and Response |
| **KQL** | Kusto Query Language for log analysis |
| **UEBA** | User and Entity Behavior Analytics |
| **Analytic Rules** | Scheduled, NRT, Fusion, ML, Threat Intel, MS Security, Anomaly |
| **STIX & TAXII** | Threat Intelligence sharing standards |
| **MITRE ATT&CK** | Adversary tactics and techniques framework |
| **Azure RBAC** | Role-Based Access Control in Azure |
| **AMA & DCR** | Azure Monitor Agent and Data Collection Rules |
| **IaC** | Infrastructure as Code with ARM Templates, Bicep, Terraform |
| **Logic Apps** | Azure-native playbook automation engine |

**Deep dives in the [`docs/concepts/`](docs/concepts/) directory.**

---

##  Getting Started

### Step 1 - Clone This Repository
```bash
git clone https://github.com/cyb-sehgal/Microsoft-Sentinel-SIEM.git
cd microsoft-sentinel-siem-lab
```

### Step 2 - Set Up Azure Prerequisites
1. Create a free [Azure Account](https://azure.microsoft.com/en-us/free/)
2. Create a **Resource Group** → [Lab 01](docs/labs/01-resource-group.md)
3. Create a **Log Analytics Workspace** → [Lab 02](docs/labs/02-log-analytics-workspace.md)
4. Deploy **Microsoft Sentinel** → [Lab 03](docs/labs/03-sentinel-deployment.md)

### Step 3 - Follow the Labs in Order
Labs are numbered and designed to build on each other. Start from Lab 01 and work your way through.

---

##  Repository Structure

```
microsoft-sentinel-siem-lab/
│
├── README.md                        ← You are here
├── docs/
│   ├── labs/                        ← Step-by-step lab guides (01–27)
│   └── concepts/                    ← Theory & concept deep-dives
│
├── queries/
│   ├── README.md                    ← KQL query index
│   ├── threat-hunting.kql           ← Threat hunting queries
│   ├── analytic-rules.kql           ← Analytic rule queries
│   └── watchlist-integration.kql    ← Watchlist KQL examples
│
└── assets/                          ← Architecture diagrams

```

---

##  Key Learnings

1. **Sentinel Deployment** - Connecting a Log Analytics Workspace to Sentinel, enabling the 31-day free trial (10 GB/day).
2. **Data Ingestion** - Connecting multiple data sources: Entra ID, Windows Security Events via AMA/DCR, and external Threat Intelligence via TAXII (Pulsedive).
3. **Analytic Rules** - Built and configured all 7 rule types: Scheduled, NRT, Fusion, ML Behavior Analytics, Threat Intelligence, Microsoft Security, and Anomaly.
4. **KQL Proficiency** - Wrote queries using `where`, `summarize`, `count`, `render`, `extend`, `project`, `distinct`, `sort`, `let`, `union`, and `join`.
5. **Threat Hunting** - Proactively hunted for Entra ID events (user creation/deletion), mapped findings to MITRE ATT&CK tactics.
6. **UEBA** - Configured User and Entity Behavior Analytics, duplicated and customized anomaly rules.
7. **SOAR Automation** - Created Automation Rules and a ChatGPT-powered Playbook using Azure Logic Apps that enriches incidents with MITRE ATT&CK tactic explanations.
8. **Attack Simulation** - Simulated SSH Brute Force attacks from Kali Linux using Hydra and Metasploit, validated detection in Defender for Cloud.
9. **Workbooks & Watchlists** - Built dashboards and integrated static IP watchlists into analytic rules.
10. **IaC & DevOps** - Explored managing Sentinel content through Azure DevOps, ARM Templates, Bicep, and Terraform.

---

##  Tools & Technologies Used

![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=flat&logo=kalilinux&logoColor=white)
![Metasploit](https://img.shields.io/badge/Metasploit-2A2A2A?style=flat&logo=metasploit&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=flat&logo=azure-devops&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat&logo=openai&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)

- **Microsoft Sentinel** - Core SIEM/SOAR platform
- **Azure Log Analytics** - Data storage and query engine
- **Microsoft Defender for Cloud** - Cloud workload protection
- **Microsoft Defender XDR** - Extended detection and response
- **Microsoft Entra ID** - Identity platform (formerly Azure AD)
- **Azure Logic Apps** - Playbook automation
- **KQL (Kusto Query Language)** - Log querying and analysis
- **Pulsedive** - External threat intelligence feed (TAXII)
- **Kali Linux + Hydra + Metasploit** - Attack simulation
- **NMAP** - Network scanning for recon simulation
- **Azure ML + MSTICPy** - Jupyter notebooks for security analysis

---

##  Notes & Limitations

- This project was completed on an **Azure Student Subscription**, which has some feature restrictions (e.g., certain Defender plans, parallel DevOps jobs).
- The **Notebook (MSTICPy)** section was partially completed due to Azure ML compute costs.
- The **Playbook with ChatGPT** lab encountered a minor issue during testing and was documented as-is for transparency.
- The **Audit Logs** connector for Entra ID had a delay in showing "Connected" - this resolved on its own after some time.
- Some Azure Portal features have been **moved to the Defender Portal** - this is noted in the relevant labs.

---

##  License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

##  Contributing

Feel free to open issues or pull requests if you find errors, have improvements, or want to add additional labs!

---

##  Acknowledgements

- [Microsoft Learn - Microsoft Sentinel Documentation](https://learn.microsoft.com/en-us/azure/sentinel/)
- [Pulsedive Community Threat Intelligence](https://pulsedive.com/)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [MSTICPy - Microsoft Threat Intelligence Python Security Tools](https://msticpy.readthedocs.io/)

---



⭐ **If this project helped you, please give it a star!** ⭐

Made with 💙 while learning Microsoft Sentinel


## Authors

- [Rahul Sehgal](https://www.github.com/cyb-sehgal)

 **Connect with me on [Linkedin](https://linkedin.com/in/rahulkram)**
