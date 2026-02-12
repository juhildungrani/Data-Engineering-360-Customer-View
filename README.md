# Project README: E-commerce Data Engineering Pipeline

This project implements a robust data engineering pipeline using a **Medallion Architecture** to process e-commerce data within a Microsoft Fabric/Synapse environment. It transforms raw data into a unified **Customer 360** view for advanced analytics.

---

## 🏗️ Architecture Overview

The pipeline is organized into three distinct layers to ensure data quality and traceability:

### 1. Bronze Layer (Raw Ingestion)

Raw data is ingested from Parquet files stored in **OneLake** and saved as Delta tables without modifications.

* **Source Path**: `abfss://ecommerce@onelake.dfs.fabric.microsoft.com/ecoomerce_lakehouse.Lakehouse/Files/Bronze/`
* **Tables Created**:
* `customers`
* `orders`
* `payments`
* `support`
* `web`



### 2. Silver Layer (Data Cleaning & Standardization)

This layer handles data quality, schema enforcement, and standardization:

* **Customer Data**: Normalizes emails to lowercase, trims whitespace, standardizes gender (Male/Female/Other), and formats locations.
* **Orders & Payments**: Handles multiple date formats (e.g., `yyyy/MM/dd`, `dd-MM-yyyy`, `yyyyMMdd`) into a unified date type.
* **Integrity Checks**: Removes duplicate IDs (e.g., `customer_id`, `order_id`) and filters out invalid records like negative payment amounts.
* **Tables Created**: `silver_customers`, `silver_orders`, `silver_payments`, `silver_support`, `silver_web`.

### 3. Gold Layer (Business Aggregation)

The final stage joins all silver tables into a single, flattened table optimized for reporting:

* **Customer360**: Aggregates demographics, order history, payment status, support ticket resolution, and web session activity.
* **Table Created**: `gold_customer360`.

---

## 🛠️ Tech Stack

* **Environment**: Microsoft Fabric / Azure Synapse
* **Language**: PySpark (Python) and Spark SQL
* **Format**: Delta Lake

---

## 🚀 How to Run

1. **Lakehouse Setup**: Ensure the e-commerce Lakehouse is configured and raw Parquet files are placed in the `/Files/Bronze/` directory.
2. **Sequential Execution**: Run the `Notebook.ipynb` cells in order.
3. **Verification**:
* Check the **Silver** tables to verify data standardization.
* Query the **Gold** table to see the final unified customer view.



---

## 📊 Sample Gold Schema

The `gold_customer360` table includes the following key fields:

* **Customer Info**: `customer_id`, `name`, `email`, `location`.
* **Transaction Info**: `order_id`, `order_amount`, `order_status`, `payment_method`.
* **Engagement Info**: `ticket_id`, `issue_type`, `page_viewed`, `device_type`.

---
