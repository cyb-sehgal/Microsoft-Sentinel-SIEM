# 🤖 Lab 18: Playbook with MITRE ATT&CK & ChatGPT

> **Requires:** OpenAI API Key

## Objective
Create an Azure Logic App Playbook that automatically enriches Sentinel incidents with MITRE ATT&CK tactic explanations using ChatGPT (OpenAI), and posts the enrichment as an incident comment.

---

## Automation Rules vs Playbooks

| Feature | Automation Rules | Playbooks |
|---|---|---|
| **Complexity** | Basic | Complex (SOAR) |
| **Engine** | Native Sentinel | Azure Logic Apps |
| **Use cases** | Assign, tag, change status, trigger playbook | Custom integrations, API calls, enrichment |
| **Triggering** | Can trigger Playbooks | Triggered by Automation Rules or manually |

---

## Prerequisites
- Microsoft Sentinel workspace
- OpenAI API Key
- Sentinel Playbook permissions configured

---

## Step 1 — Configure Playbook Permissions

1. Go to **Sentinel** → **Settings** → **Playbook permissions**
2. Click **Configure Permissions**
3. Click **Browse** → select your Resource Group → **Apply**

---

## Step 2 — Create the Playbook

1. Go to **Automation** → **Create** → **Playbook with incident trigger**
2. Configure:
   - **Resource Group:** Your resource group
   - **Playbook Name:** `ChatGPT`
3. Leave rest as default → **Create Playbook**
4. You'll be redirected to **Logic App Designer**

---

## Step 3 — Verify Sentinel Connection

1. Click the **Sentinel connector** button in the designer
2. Click **Change Connection** — verify it's connected

---

## Step 4 — Enable Managed Identity

1. Go to the Logic App → **Identity**
2. Ensure **System Assigned** status is **On**
3. Click **Azure role assignments** → **Add role assignment**
4. Assign the appropriate Sentinel role → **Save**

---

## Step 5 — Add ChatGPT Action

1. In Logic App Designer, click **+** → **Add an action**
2. Search for **"GPT Complete your prompt"** → select it
3. In the popup:
   - Rename the action
   - Paste your OpenAI API Key in the format shown
   - Click **Create**
4. In the prompt field, enter:

```
Please enrich the Incident with an explanation of the MITRE ATT&CK Tactic
```

5. After the prompt, add a **Dynamic Content** reference to the incident's **Tactic** field

---

## Step 6 — Add Comment Action

1. Click **+** → **Add an action** for posting a comment
2. In **Incident ARM ID**: Click **Insert Expression** → search for `ARM ID` → select **Incident ARM ID**
3. In **Incident comment message**: Click **Insert Expression** → search for `Text` → select it → **Add**

---

## Step 7 — Save and Test

1. Click **Save** in the top left corner
2. Go to **Sentinel** → **Incidents** → select any incident
3. Click **Action** → **Run Playbook**
4. Select your `ChatGPT` playbook → **Run**
5. Refresh the incident — the ChatGPT enrichment comment should appear

---

## Azure Logic App Connectors

Logic Apps connect to hundreds of services via connectors including:
- Microsoft Sentinel
- Microsoft Teams
- ServiceNow
- Slack
- PagerDuty
- Email (Office 365, Gmail)
- OpenAI / ChatGPT
- And hundreds more

---

## Notes

> ⚠️ During the lab, a configuration issue was encountered during the testing phase. The playbook configuration was documented as completed — further debugging would be required in a production environment. This is a known challenge when working with Logic App managed identity permissions.
