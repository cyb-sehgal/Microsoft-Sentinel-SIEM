# 🏗️ Infrastructure as Code (IaC) & Sentinel Repositories

## What is Infrastructure as Code?

Infrastructure as Code (IaC) is the practice of managing and provisioning infrastructure through code instead of manual processes.

### Benefits
- ✅ Reduced human errors
- ✅ Increased deployment speed at scale
- ✅ Improved deployment consistency
- ✅ Cost reduction
- ✅ Security & governance by design

---

## Managing Sentinel via Repositories

Repositories allow you to automate the deployment and management of your Sentinel content through central repositories with IaC.

### Supported Repository Types
- **GitHub**
- **Azure DevOps**

### Supported Content Types
1. Analytics Rules
2. Hunting Queries
3. Automation Rules
4. Playbooks
5. Parsers
6. Workbooks

---

## IaC Options for Sentinel

### ARM Templates (Azure Resource Manager)
- Native Azure IaC format
- JSON-based
- Supports all Sentinel resource types

### Azure Bicep
- A domain-specific language (DSL) for ARM templates
- More readable and concise syntax than raw ARM JSON
- Transpiles to ARM templates

### Terraform (HashiCorp)
- Cloud-agnostic IaC tool
- Large community and module ecosystem
- `azurerm` provider supports Sentinel resources

---

## Azure DevOps + Sentinel

Azure DevOps provides CI/CD pipelines that can automate Sentinel content deployment:

1. Store Sentinel content (analytics rules, playbooks, etc.) as code in a DevOps repo
2. Use pipelines to deploy changes automatically on merge/commit
3. Enables version control, peer review, and rollback for Sentinel configurations

### Lab Setup Summary

1. Create an Azure DevOps organization at [dev.azure.com](https://dev.azure.com)
2. Create a new project (set to Private)
3. Enable third-party OAuth: **Org Settings** → **Policy** → **Third-party application access via OAuth** → Turn On
4. Set up billing for Parallel Jobs: **Parallel Jobs** → **Microsoft-Hosted** → Change 0 to 1
5. Connect Sentinel to the DevOps repository via the Repositories section in Sentinel

> ⚠️ **Note:** Azure Student Subscriptions may have limitations with DevOps parallel job billing. This was encountered during the lab.
