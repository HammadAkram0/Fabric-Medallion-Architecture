# 🪶 Medallion Architecture in Microsoft Fabric

This project demonstrates how to build an **end-to-end Medallion Architecture** using **Microsoft Fabric** — moving data seamlessly from raw to analytics-ready layers using **Lakehouse, Data Pipelines, and Notebooks**.

---

## 🧱 Architecture Overview

The **Medallion Architecture** organizes data into structured layers for cleaner, faster, and more reliable analytics.

| Layer | Purpose | Example |
|-------|----------|----------|
| 🥉 Bronze | Raw, unprocessed data landed directly into Lakehouse | Source CSVs from storage |
| 🥈 Silver | Standardized and cleaned data | PySpark transformations |
| 🥇 Gold | Curated, business-ready data for reporting | Fact & dimension tables for Power BI |

![Architecture Overview](./Screenshots/medallion_layers.png)

---

## ⚙️ Fabric Components Used
- **Data Pipelines** → to orchestrate ingestion and transformations  
- **Notebooks (PySpark)** → for data cleaning, enrichment, and Delta Lake merges  
- **Lakehouse** → unified storage for bronze, silver, and gold tables  
- **Power BI Semantic Model** → to build reports on gold data

---

## 🧩 Pipeline Workflow
1. **Raw_Staging Notebook**
   - Reads raw CSVs from `Files/bronze/`
   - Applies schema and loads data into Bronze table

2. **Standardized_Data Notebook**
   - Cleans nulls and flags old records
   - Writes standardized data to Silver table using Delta Merge

3. **Analytics_Ready Notebook**
   - Prepares fact and dimension tables (Gold)
   - Publishes clean tables to the Lakehouse for Power BI

![Pipeline Success](./Screenshots/pipeline_run_success.png)

---

## 🧠 Sample PySpark Code

```python
from pyspark.sql.types import *

# Define schema
orderSchema = StructType([
    StructField("SalesOrderNumber", StringType()),
    StructField("SalesOrderLineNumber", IntegerType()),
    StructField("OrderDate", DateType()),
    StructField("CustomerName", StringType()),
    StructField("Email", StringType()),
    StructField("Item", StringType()),
    StructField("Quantity", IntegerType()),
    StructField("UnitPrice", FloatType()),
    StructField("Tax", FloatType())
])

# Load raw data from Bronze layer
df = spark.read.format("csv").option("header", "true").schema(orderSchema).load("Files/bronze/*.csv")
