# 📊 KQL Query Reference

This directory contains all the Kusto Query Language (KQL) queries used throughout the Microsoft Sentinel SIEM Lab project.

## Files

| File | Description |
|------|-------------|
| [threat-hunting.kql](threat-hunting.kql) | Queries for proactive threat hunting |
| [analytic-rules.kql](analytic-rules.kql) | Queries used in Analytic Rules |
| [watchlist-integration.kql](watchlist-integration.kql) | Queries integrating Watchlists |

---

## KQL Operators Quick Reference

| Operator | Action |
|---|---|
| `where` | Filters on a specific predicate |
| `search` | Searches all columns in the table for the value |
| `limit` / `take` | Returns the specified number of records |
| `count` | Counts records in the input table |
| `summarize` | Groups rows by columns and calculates aggregations |
| `render` | Renders results as graphical output (piechart, timechart, etc.) |
| `extend` | Creates a calculated column and adds it to the result set |
| `project` | Selects the columns to include in the specified order |
| `distinct` | Produces a table with distinct combinations of provided columns |
| `sort` | Sorts the rows of the input table by one or more columns |
| `let` | Creates a temporary variable that can be referenced |
| `union` | Takes two or more tables and returns all their rows |
| `join` | Merges rows of two tables by matching values of specified columns |
