# 📋 Lab 20: Watchlists & Analytic Rule Integration

## What are Watchlists?

Watchlists are **static lists** that can be referenced in KQL queries within Sentinel. They allow you to enrich detections with context (e.g., known malicious IPs, VIP users, critical assets).

### Common Watchlist Use Cases

| Use Case | Description |
|---|---|
| **VIPs** | Flag activity involving high-value users |
| **Former employees** | Detect access from ex-employees' accounts |
| **Lost & stolen devices** | Alert when these assets connect |
| **Exceptions** | Suppress known-good activity from alerting |
| **Critical assets** | Prioritize alerts involving critical systems |
| **Pentest IPs** | Suppress alerts from authorized pentest infrastructure |

---

## Lab Part 1: Creating a Watchlist

### Steps

1. In Sentinel, go to **Watchlist** section → click **New**
2. Fill in the details:
   - **Name:** `PentestingIPAddresses`
   - **Alias:** `PentestingIPAddresses`
3. Click **Next: Source**
4. Select **Local File** and upload your CSV file
   - CSV format: one column named `IPAddress`, one IP per row
5. Click **Review + Create** → **Create**

> ⏳ It may take a few minutes for the Watchlist to be fully available in the Portal.

### Verifying the Watchlist in Logs

```kql
_GetWatchlist('PentestingIPAddresses')
| project IPAddress
```

---

## Lab Part 2: Updating a Watchlist

1. Navigate to your Watchlist → **Update watchlist** → **Edit watchlist items**
2. Add new IPs manually (e.g., `8.8.8.8` for Google DNS)
3. Click **Save**

> Updates may take up to 10 minutes to reflect in queries.

---

## Lab Part 3: Integrating Watchlists with Analytic Rules

### Steps

1. Go to **Sentinel Content Hub** → search **"High count"**
2. Install: **High count of connection by client IP on many ports**
3. Navigate to **Analytics** → **Rule Templates** → **High count of conn...**
4. Click **Create Rule** → proceed through the wizard
5. In the **Set rule logic** section, add this line to the **top** of the KQL query:

```kql
let PentestingIPAddresses = _GetWatchlist('PentestingIPAddresses') | project IPAddress;
```

6. This will allow the rule to reference the watchlist and filter/flag accordingly
7. Leave remaining settings as default → **Review + Create** → **Save**

✅ The rule is now active and integrated with your Watchlist.

---

## Key Takeaway

Watchlists allow you to make your analytic rules **context-aware** without hardcoding values in KQL. Update the watchlist CSV, and all referencing rules automatically benefit — no rule editing required.
