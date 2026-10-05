# 💰 Data Cloud Cost Optimization (FinOps Toolkit)

A collection of SQL queries, Python scripts, and architectural guidelines to audit, identify, and reduce compute & storage costs in Cloud Data Warehouses (BigQuery & Snowflake).

## 🎯 What's Inside

- **BigQuery Cost Analyzer:** SQL scripts using `INFORMATION_SCHEMA.JOBS_BY_PROJECT` to detect top 10 most expensive queries, unpartitioned table scans, and idle slots.
- **Snowflake Warehouse Optimization:** Queries on `ACCOUNT_USAGE.QUERY_HISTORY` to detect long-running queries, auto-suspend misconfigurations, and disk spillage.
- **FinOps Best Practices Checklist:** A practical markdown guide for data teams to prevent budget overruns.

## 🚀 Quick Start (BigQuery Example)

To identify queries that scanned over 100GB in the last 7 days:

```sql
SELECT
  project_id,
  user_email,
  query,
  ROUND(total_bytes_billed / 1024 / 1024 / 1024, 2) AS gb_billed,
  ROUND((total_bytes_billed / 1024 / 1024 / 1024 / 1024) * 6.25, 2) AS estimated_cost_usd
FROM
  `region-us`.INFORMATION_SCHEMA.JOBS_BY_PROJECT
WHERE
  creation_time >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
ORDER BY
  total_bytes_billed DESC
LIMIT 10;
