# 📋 FinOps & Data Cloud Cost Optimization Checklist

A practical checklist for Data Engineers and Analytics Teams to audit, optimize, and prevent budget overruns in Cloud Data Warehouses (BigQuery, Snowflake, Azure Databricks, Redshift).

---

### 1. Query & Code Optimization ⚡
- [ ] **Avoid `SELECT *`:** Query only the explicit columns required to reduce data scanned and compute overhead.
- [ ] **Leverage Partitioning & Clustering:** Ensure queries filter on partition keys (e.g., `date_col`) to avoid full table scans.
- [ ] **Optimize Joins:** Join on indexed or clustered keys and filter data *before* applying `JOIN` operations.
- [ ] **Use Approximate Functions:** For high-cardinality metrics, use `APPROX_COUNT_DISTINCT()` instead of `COUNT(DISTINCT)` where appropriate.
- [ ] **Materialize Repeated Queries:** Convert complex, frequently executed CTEs or subqueries into Materialized Views or incremental dbt models.

---

### 2. Warehouse & Compute Management 🖥️
- [ ] **Auto-Suspend Rules:** Set aggressive auto-suspend timeouts on virtual warehouses (e.g., 60–120 seconds in Snowflake).
- [ ] **Right-Sizing Compute:** Start with the smallest cluster/warehouse size and scale up only when SLA requirements demand it.
- [ ] **Separate Workloads:** Isolate heavy ETL/ELT pipelines from BI/Ad-hoc reporting warehouses to prevent compute contention.
- [ ] **Query Timeout Limits:** Configure maximum execution time limits to automatically terminate runaway or deadlocked queries.

---

### 3. Storage & Data Lifecycle Management 📦
- [ ] **Partition Expiration Policies:** Set expiration days for temporary, staging, or dev/test tables.
- [ ] **Cold Storage Tiering:** Move historical or raw log data to cheaper archive storage tiers (e.g., AWS S3 Glacier, GCS Archive, Azure Cool/Cold Blob).
- [ ] **Clean Up Unused Tables:** Periodically audit and drop orphaned staging tables or unused sandbox datasets.
- [ ] **Time Travel / Fail-safe Tuning:** Adjust fail-safe and time-travel retention periods according to table criticality (e.g., shorter retention for transient tables).

---

### 4. Monitoring & Governance 🚨
- [ ] **Budget Alerts:** Configure daily and monthly cost threshold alerts in cloud billing consoles.
- [ ] **Resource Tagging:** Enforce strict tagging standards (e.g., `environment`, `team`, `project_id`) on compute resources for cost attribution.
- [ ] **Identify Top Spenders:** Run weekly audits on system logs (`INFORMATION_SCHEMA` / `ACCOUNT_USAGE`) to track top 10 most expensive queries and users.
