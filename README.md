# 📊 Data Pipeline Project (Databricks ETL)

## 🚀 Overview

This project demonstrates an end-to-end **ETL (Extract, Transform, Load) pipeline** built using Databricks.
The pipeline processes raw CSV data from multiple vendors, cleans and standardizes it, and produces a final aggregated **daily sales dataset**.

---

## 🧱 Architecture

The pipeline follows the **Medallion Architecture**:

```
Bronze (Raw Data) → Silver (Clean Data) → Gold (Aggregated Data)
```

---

## 📂 Project Structure

```
data-pipeline-project/
│
├── notebooks/
│   ├── 01_ingestion.py        # Bronze Layer (Data Ingestion)
│   ├── 02_transformation.py   # Silver Layer (Data Cleaning)
│   ├── 03_aggregation.py      # Gold Layer (Aggregation)
│
├── output/
│   └── final_sales.csv        # Final output dataset
│
├── README.md
├── requirements.txt
```

---

## 📥 Data Source

* Multiple CSV files from different vendors
* Stored in **Databricks Unity Catalog Volume**
* Data contains inconsistencies such as:

  * Missing values
  * Incorrect formats
  * Mixed data types

---

## ⚙️ Pipeline Steps

### 🥉 1. Ingestion (Bronze Layer)

* Read multiple CSV files from volume
* Combine all vendor data into a single dataset
* Add file-level metadata for tracking

**Output Table:**

```
assignment_catalog.bronze_layer.sales_raw
```

---

### 🥈 2. Transformation (Silver Layer)

* Standardize schema across all vendors
* Handle invalid data using safe casting (`try_cast`)
* Convert data types (string → int/double/date)
* Calculate revenue:

```
revenue = qty_sold × mrp
```

* Extract vendor name from file path

**Output Table:**

```
assignment_catalog.silver_layer.sales_clean
```

---

### 🥇 3. Aggregation (Gold Layer)

* Group data by:

  * date
  * product identifier (sku)
  * data source

* Compute:

  * total_units
  * total_revenue

**Output Table:**

```
assignment_catalog.gold_layer.daily_sales
```

---

## 📤 Final Output

The final dataset is exported as:

```
output/final_sales.csv
```

---

## ✅ Final Schema

| Column             | Description             |
| ------------------ | ----------------------- |
| date               | Transaction date        |
| product_identifier | Product SKU             |
| total_units        | Total units sold        |
| total_revenue      | Total revenue generated |
| data_source        | Vendor name             |

---

## 🔄 Workflow

Pipeline execution order:

```
01_ingestion → 02_transformation → 03_aggregation
```

* Each step depends on the previous
* Can be scheduled using Databricks Workflows

---

## 🧠 Key Features

* Multi-source data ingestion
* Schema standardization
* Data validation and cleaning
* Aggregation for business insights
* Built using Spark and Delta Lake

---

## ⚠️ Challenges & Solutions

| Challenge                         | Solution                          |
| --------------------------------- | --------------------------------- |
| Invalid date values               | Used `try_cast` to avoid failures |
| Numeric values as strings ("1.0") | Converted via double → int        |
| Schema mismatch                   | Used `overwriteSchema`            |
| Storage limitations               | Used Unity Catalog Volumes        |

---

## 💬 Conclusion

This project demonstrates how to design and implement a **scalable ETL pipeline** using Databricks. It handles real-world data challenges such as inconsistent schemas and data quality issues while producing a clean, analysis-ready dataset.

---

## 👨‍💻 Author

**Abhay Soni**
