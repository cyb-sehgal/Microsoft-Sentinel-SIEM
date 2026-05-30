# 🌐 Cyber Threat Intelligence in Sentinel

## STIX & TAXII Overview

### STIX (Structured Threat Information Expression)
- A **standardized language** built on JSON
- Enables the exchange of Cyber Threat Intelligence (CTI) between systems
- Describes threat actors, malware, attack patterns, indicators of compromise (IOCs), and more

### TAXII (Trusted Automated Exchange of Intelligence Information)
- The **protocol** that transmits STIX data between systems
- Transmitted via **HTTPS**
- TAXII servers expose collections of threat intelligence that clients can pull

---

## Threat Intelligence in Sentinel

Threat Intelligence can be used across multiple Sentinel features:

| Feature | Use |
|---|---|
| **Analytic Rules** | Match IOCs against ingested logs to generate alerts |
| **Threat Hunting** | Proactively search for known malicious indicators |
| **Incidents** | Enrich incidents with threat intelligence context |
| **Workbooks** | Visualize threat indicator data |
| **Notebooks** | Python-based analysis of threat intel data |
| **Playbooks** | Automate responses when IOCs are detected |

---

## Lab: Ingesting External Threat Intel via TAXII (Pulsedive)

### Steps Summary
1. Go to **Sentinel Content Hub** (now in Defender Portal) → search **Threat Intelligence** → Install
2. After installation, navigate to **Data Connectors** → **Threat Intelligence - TAXII**
3. Use [Pulsedive Community](https://pulsedive.com/) as the external threat feed
4. After signing up at Pulsedive, retrieve your **API Key** from your account settings
5. Visit [Pulsedive Quick Setup Docs](https://pulsedive.com/docs) for:
   - **API Root URL**
   - **Collection ID**
   - Username and Password
6. In the TAXII connector form:
   - Username: `taxii2`
   - Password: your Pulsedive API Key
7. Submit — the connector is now active

### Verification
```kql
// Check for ingested threat indicators
ThreatIntelligenceIndicator
| take 100
```

---

## Ingesting Entra ID Threat Data

1. In Defender Portal Content Hub, search **Microsoft Entra ID** → Install
2. Go to **Data Connectors** → **Microsoft Entra ID** → Open Connector Page
3. Select **Audit Logs** → Apply Changes

### Note on Audit Log Delays
Audit Logs may take time to show as "Connected." This is expected behavior — the connector updates periodically. Use the Logs section to verify ingestion:

```kql
AADNonInteractiveUserSignInLogs
| take 100
```
