# Day 4 Completed — Delta Lake Introduction (Databricks 14 Days AI Challenge)

Today I practiced **Delta Table** on an e-commerce events dataset in Databricks.

---

## 📌 Dataset Used
**File:** `2019-Oct.csv` , `2019-Nov.csv`
**Columns:** `event_time, event_type, product_id, category_id, category_code, brand, price, user_id, user_session`

---

## 📘 What I Learned Today
- What is Delta Lake?
- ACID transactions
- Schema enforcement
- Delta vs Parquet

---

## 🛠️ Tasks I Completed
1. Convert CSV to Delta format
2. Create Delta tables (SQL and PySpark)
3. Test schema enforcement
4. Handle duplicate inserts

---

##  Practice Queries (Beginner Version)

#### Convert to Delta
events.write.format("delta").mode("overwrite").save("/delta/events")

#### Create managed table
events.write.format("delta").saveAsTable("events_table")

#### SQL approach
spark.sql("""
    CREATE TABLE events_delta
    USING DELTA
    AS SELECT * FROM events_table
""")

#### Test schema enforcement
try:
    wrong_schema = spark.createDataFrame([("a","b","c")], ["x","y","z"])
    wrong_schema.write.format("delta").mode("append").save("/delta/events")
except Exception as e:
    print(f"Schema enforcement: {e}")

## Notebooks

![Day 4 File](../submissions/day04/Day4.ipynb)

## Screenshots

![Day 4 File](../submissions/day04/Day4a.png)

![Day 4 File](../submissions/day04/Day4b.png)

![Day 4 File](../submissions/day04/Day4c.png)

