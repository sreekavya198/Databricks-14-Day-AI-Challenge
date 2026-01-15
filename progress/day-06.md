# Day 6 Completed — Medallion Architecture (Bronze → Silver → Gold)

Today I learned the **Medallion Architecture** pattern in Databricks and implemented a simple 3-layer pipeline using **Delta Lake**.

---

## 📘 What I Learned Today
- **Bronze**: raw ingestion (minimal changes, add metadata)
- **Silver**: cleaned + validated data (dedupe, filters, derived columns)
- **Gold**: business-ready aggregates (KPIs for reporting)
- Why splitting layers improves reliability, debugging, and reusability

---

## 🛠️ Tasks I Completed
1. Designed a 3-layer pipeline
2. Built **Bronze**: raw ingestion + ingestion timestamp
3. Built **Silver**: cleaning, validation, dedup, derived features
4. Built **Gold**: business aggregates (views, purchases, revenue, conversion rate)

---

#### Screenshots
![Day 6 screenshot](../assets/day-06/ss1.png)
![Day 6 screenshot](../assets/day-06/ss2.png)
![Day 6 screenshot](../assets/day-06/ss3.png)
