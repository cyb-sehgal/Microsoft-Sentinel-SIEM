# 📊 KQL Fundamentals — Kusto Query Language

## What is KQL?

KQL (Kusto Query Language) is the query language used in Microsoft Sentinel (and Azure Monitor) to search, filter, and analyze log data stored in Log Analytics Workspaces.

---

## KQL Query Development Process

```
1. Hypothesis      → What are you trying to prove or disprove?
                     Example: "Is this IP address in my logs?"

2. Determine       → Which table contains the data you need?
   Tables            Example: SecurityEvent, AuditLogs, ThreatIntelligenceIndicator

3. Explore Schema  → Understand the columns available in that table

4. Filter Away     → Use where, search, and other operators to narrow results

5. Visualize       → Use render to display charts and graphs
```

---

## KQL Structure

```kql
SecurityEvent                       // Table  — What data source?
| where EventID == "4264"           // Filter — What condition?
| summarize count() by Account      // Aggregation
| order by Account asc              // Ordering
```

---

## Core Operators with Examples

### `where` — Filter rows
```kql
SecurityEvent
| where EventID == "4688"
| where TimeGenerated > ago(3d)
```

### `search` — Search all columns
```kql
search "administrator"
```

### `limit` / `take` — Return N rows
```kql
SecurityEvent
| take 10
```

### `count` — Count rows
```kql
SecurityEvent
| where EventID == "4688"
| count
```

### `summarize` — Aggregate data
```kql
SecurityEvent
| summarize count() by Computer
```

### `render` — Visualize
```kql
SecurityEvent
| summarize count() by bin(TimeGenerated, 1h)
| render timechart

SecurityEvent
| summarize count() by Computer
| render barchart

SecurityEvent
| summarize count() by EventID
| render piechart
```

### `extend` — Add a calculated column
```kql
SecurityEvent
| extend DayOfWeek = dayofweek(TimeGenerated)
```

### `project` — Select specific columns
```kql
SecurityEvent
| where EventID == "4688"
| project TimeGenerated, Computer, Account, CommandLine
```

### `project-away` — Remove specific columns
```kql
SecurityEvent
| project-away TenantId, MG
```

### `distinct` — Unique values
```kql
SecurityEvent
| distinct Account
```

### `sort` — Order results
```kql
SecurityEvent
| summarize count() by Account
| sort by count_ desc
```

### `let` — Define a variable
```kql
let threshold = 10;
SecurityEvent
| summarize count() by Computer
| where count_ > threshold
```

### `union` — Combine tables
```kql
union SecurityEvent, AuditLogs
| where TimeGenerated > ago(1d)
| take 100
```

### `join` — Join two tables
```kql
SecurityEvent
| where EventID == "4688"
| join kind=inner (
    SecurityEvent
    | where EventID == "4624"
) on Account
```

---

## Time Functions

```kql
| where TimeGenerated > ago(1h)     // Last 1 hour
| where TimeGenerated > ago(1d)     // Last 1 day
| where TimeGenerated > ago(7d)     // Last 7 days
| where TimeGenerated > ago(30d)    // Last 30 days
```

---

## Tips

- Press `Shift + Enter` to run a query in the Log Analytics query editor
- Use the **Time Range** selector to scope queries to a specific window
- Click the dropdown arrow on results to see detailed event views
- The **Tables** panel (left sidebar) shows all available tables in your workspace
