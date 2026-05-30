# 🔍 Analytic Rule Types in Microsoft Sentinel

Analytic Rules are your SIEM use-cases defined via KQL. Sentinel comes with over **500 rule templates** and supports a limit of **512 active rules per workspace**.

---

## The 7 Types of Analytic Rules

### 1. 📅 Scheduled Rules
- Analytic Rules that continuously run on a defined timeframe
- An alert is fired if the condition is met
- The default and most common rule type
- **Most analytic rules use this type**
- Time frame and frequency are fully customizable

### 2. ⚡ Near-Real-Time (NRT) Rules
- Run continuously and provide "up-to-the-minute" threat detections
- Run once every minute in reality
- **Limit: 50 NRT rules per workspace**
- Best for high-priority detections requiring minimal delay

### 3. 🔗 Fusion Rules
- Advanced **multistage attack detection** feature
- Correlates signals across 120+ detections from multiple Microsoft data sources:
  - Entra ID Identity Protection
  - Defender for Cloud
  - Defender for IoT
  - Microsoft 365 Defender
  - And more
- **Only one Fusion rule allowed per Sentinel workspace**
- Cannot be customized significantly — managed by Microsoft

### 4. 🤖 ML Behavior Analytics Rules
- Monitors for unusual **Windows RDP** and **Linux SSH** logon patterns
- Based on pre-defined ML scenarios:
  - **Unusual IP** — IP not seen in last 30 days
  - **Unusual Geo** — IP, city, country, or ASN not seen in last 30 days
  - **New User** — User logs in from unexpected IP and geolocation
- Takes 7 days to build a baseline behavioral profile after enabling

### 5. 🌐 Threat Intelligence Rules
- Generates alerts when a **Microsoft Defender Threat Intelligence indicator** matches your event logs
- Very high-fidelity alerts
- Limited customization — primarily toggle on/off

### 6. 🛡️ Microsoft Security Rules
- Alert **forwarders** from other Microsoft security services:
  - Defender for Endpoint
  - Defender for Identity
  - Defender for Cloud
- Created via the "Microsoft incident creation rule" option
- Automatically creates Sentinel incidents from Defender alerts

### 7. 📈 Anomaly Rules
- Statistical-based anomaly detection
- Built-in rules managed by Microsoft
- Can be **duplicated** to create custom anomaly rules with modified thresholds
- Anomaly Score Threshold can be adjusted (e.g., 0.8 = 80%)
- Custom rules run in "Flighting" mode initially; can be switched to Production

---

## Key Notes

- Rule templates in the **Rule Templates** tab do not count toward the 512 rule limit — only active/deployed rules do.
- Use the **Filter** tab to narrow templates by Data Source, Rule Type, etc.
- Most rules from the Content Hub are Scheduled rules.
