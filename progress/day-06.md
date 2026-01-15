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
## Practice

#### BRONZE: Raw ingestion
raw = spark.read.csv("/raw/events.csv", header=True, inferSchema=True)
raw.withColumn("ingestion_ts", F.current_timestamp()) \
   .write.format("delta").mode("overwrite").save("/delta/bronze/events")

#### SILVER: Cleaned data
bronze = spark.read.format("delta").load("/delta/bronze/events")
silver = bronze.filter(F.col("price") > 0) \
    .filter(F.col("price") < 10000) \
    .dropDuplicates(["user_session", "event_time"]) \
    .withColumn("event_date", F.to_date("event_time")) \
    .withColumn("price_tier",
        F.when(F.col("price") < 10, "budget")
         .when(F.col("price") < 50, "mid")
         .otherwise("premium"))
silver.write.format("delta").mode("overwrite").save("/delta/silver/events")

#### GOLD: Aggregates
silver = spark.read.format("delta").load("/delta/silver/events")
product_perf = silver.groupBy("product_id", "product_name") \
    .agg(
        F.countDistinct(F.when(F.col("event_type")=="view", "user_id")).alias("views"),
        F.countDistinct(F.when(F.col("event_type")=="purchase", "user_id")).alias("purchases"),
        F.sum(F.when(F.col("event_type")=="purchase", "price")).alias("revenue")
    ).withColumn("conversion_rate", F.col("purchases")/F.col("views")*100)
product_perf.write.format("delta").mode("overwrite").save("/delta/gold/products")


## Notebooks

![Day 6 File](../submissions/day06/Day6.ipynb)

## Screenshots

![Day 6 File](../submissions/day06/Day6a.png)

![Day 6 File](../submissions/day06/Day6b.png)

![Day 6 File](../submissions/day06/Day6c.png)
