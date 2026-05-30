# 👤 User and Entity Behavior Analytics (UEBA)

## What is UEBA?

User and Entity Behavior Analytics (UEBA) is a Sentinel feature that builds behavioral baselines for users and entities (devices, apps, services) — then flags deviations from those baselines as potential threats.

UEBA uses machine learning to detect:
- Unusual sign-in patterns
- Impossible travel
- Anomalous data access
- Privilege escalation indicators

---

## UEBA Architecture

```
Data Sources (Entra ID, Windows Events, etc.)
        │
        ▼
  Log Analytics Workspace
        │
        ▼
  UEBA Engine (ML baseline modeling)
        │
        ▼
  Anomaly Alerts → Sentinel Incidents
```

---

## Lab: Configuring UEBA

### Step 1 — Enable UEBA
1. In Sentinel, go to **Threat Management** → **Entity Behavior**
2. Click **Entity Behavior Settings**
3. Click **Set UEBA settings**
4. Toggle **1. Turn on the UEBA feature**
5. Check **Microsoft Entra ID** and hit **Apply**
6. Do the same for **Audit Logs**

### Step 2 — Review Anomaly Rules
1. Go to **Analytics** → **Configuration** section
2. Filter by **Data Sources** → **Microsoft Entra ID** → **Apply**
3. Anomaly rules associated with Entra ID will be listed
4. Many are enabled by default

### Step 3 — Customize Anomaly Rules

Since built-in Microsoft anomaly rules are read-only, you can duplicate them to customize:

1. Select an anomaly rule (e.g., **Anomalous Account Creation**)
2. Right-click → **Duplicate**
3. The duplicate will have a **FLGT** (Flighting) tag and be **Disabled**
4. Select the duplicate → **Edit**
5. **Enable** it and switch mode to **Production**
6. Adjust the **Anomaly Score Threshold** (e.g., `0.8` = 80%)
7. Click **Review + Create** → wait for validation → **Save**

---

## Notes

- UEBA requires **Entra ID** as a minimum data source
- After enabling UEBA, allow time for the engine to build behavioral profiles
- Anomalies are visible in the **Anomalies** tab under Analytics
- Custom (duplicated) anomaly rules appear alongside the built-in ones once activated
