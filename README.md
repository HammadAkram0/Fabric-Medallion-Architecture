# Medallion Architecture in Microsoft Fabric

An end-to-end medallion architecture in Microsoft Fabric. Raw files move through Bronze, Silver and Gold layers using Lakehouse storage, Data Pipelines and PySpark notebooks, ending in analytics-ready tables for Power BI.

**Stack:** Microsoft Fabric | Lakehouse | Data Pipelines | PySpark | Delta Lake | Power BI

## Architecture

| Layer | Purpose | In this project |
|---|---|---|
| Bronze | Raw, unprocessed data landed as-is | Source CSVs loaded from `Files/bronze/` |
| Silver | Cleaned and standardised data | PySpark transformations and Delta merges |
| Gold | Curated, business-ready data | Fact and dimension tables for Power BI |

## Fabric components used

- **Data Pipelines** orchestrate ingestion and the transformation steps
- **Notebooks (PySpark)** handle cleaning, enrichment and Delta Lake merges
- **Lakehouse** stores the Bronze, Silver and Gold tables
- **Power BI semantic model** builds reports on the Gold layer

## Pipeline workflow

1. **Raw staging:** reads the raw CSVs from `Files/bronze/`, applies a schema and loads a Bronze table.
2. **Standardised data** ([`Transform data for Silver.ipynb`](Notebook/Transform%20data%20for%20Silver.ipynb)): cleans nulls, flags old records and writes the Silver table with a Delta merge.
3. **Analytics ready** ([`Transform data for Gold.ipynb`](Notebook/Transform%20data%20for%20Gold.ipynb)): prepares fact and dimension tables and publishes them to the Lakehouse for Power BI.

## Sample code

```python
from pyspark.sql.types import *

# Define the schema
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

# Load raw data from the Bronze layer
df = spark.read.format("csv").option("header", "true").schema(orderSchema).load("Files/bronze/*.csv")
```

## Repository structure

```
.
|-- Notebook/
|   |-- Transform data for Silver.ipynb
|   `-- Transform data for Gold.ipynb
`-- README.md
```

## Related project

For a fuller Fabric solution that adds ingestion from SQL Server and SharePoint, a star schema and a Power BI dashboard, see [Microsoft-Fabric-WWI-Data-Engineering-Project](https://github.com/HammadAkram0/Microsoft-Fabric-WWI-Data-Engineering-Project).

## Author

Hammad Akram, Data Analytics Engineer | [Portfolio](https://hammadakram.vercel.app) | [LinkedIn](https://www.linkedin.com/in/hammadakram0/)
