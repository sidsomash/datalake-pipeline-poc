
---

# **Online Retail Lakehouse Project**

**Author:** Sid Somashekar  
**Version:** 0.1  
**Dataset:** UCI Online Retail (541,909 rows, 8 columns)  
**Tech Stack:** PySpark, Delta Lake, Airflow, Python, DuckDB (optional), MinIO (optional)  
**Goal:** Build a fully open‑source medallion lakehouse pipeline demonstrating SQL fluency, Spark fluency, pipeline engineering, governance, validation, and architecture maturity.

---

## 1. Overview

This project implements a governed medallion lakehouse using the UCI Online Retail dataset.  
It simulates an enterprise data engineering environment with:

- Raw ingestion  
- Cleaning and standardization  
- Relational modeling  
- SCD Type 2 dimensions  
- Delta Lake merges  
- Spark SQL + DataFrame transformations  
- Airflow orchestration  
- Validation and QA  
- Documentation and governance  

The project is designed to strengthen:

- Spark proficiency  
- SQL proficiency (CTEs, temp tables, window functions)  
- Pipeline design  
- Architecture communication  
- Debugging and optimization  

It is also designed to be AI‑agent friendly for Copilot CLI, Claude Code, Gemini Code Assist, and other agentic tools.

---

## 2. Architecture

### 2.1 Medallion Architecture

```
Bronze (raw ingestion)
    ↓
Silver (cleaned, relational, validated)
    ↓
Gold (analytics, business-ready)
```

### 2.2 Logical Data Model (Post-Silver)

**Tables**

- `customers`
- `products`
- `orders`
- `order_items`
- `countries`

**Relationships**

- `orders.customer_id → customers.customer_id`
- `order_items.invoice_no → orders.invoice_no`
- `order_items.stock_code → products.stock_code`
- `customers.country → countries.country_name`

---

## 3. Dataset Schema (Raw)

| Column      | Description |
|-------------|-------------|
| InvoiceNo   | Transaction ID (cancellations start with “C”) |
| StockCode   | Product ID |
| Description | Product name |
| Quantity    | Quantity purchased |
| InvoiceDate | Timestamp |
| UnitPrice   | Price per unit |
| CustomerID  | Customer ID |
| Country     | Customer country |

---

## 4. Tech Stack

### Core
- PySpark  
- Delta Lake  
- Python 3.10+

### Orchestration
- Apache Airflow

### Optional
- DuckDB (SQL prototyping)  
- MinIO (S3 simulation)  
- Great Expectations (validation)

---

## 5. Pipeline Stages

### 5.1 Bronze Layer
- Ingest raw Excel → CSV/Parquet  
- Infer schema  
- Normalize timestamps  
- Handle cancellations  
- Write Bronze Delta tables partitioned by `InvoiceDate`

### 5.2 Silver Layer
- Clean data using CTEs + temp views  
- Split into relational tables  
- Deduplicate  
- Validate Quantity × UnitPrice  
- Write Silver Delta tables

### 5.3 Gold Layer
- RFM segmentation  
- Customer lifetime value  
- Product performance  
- Country-level analytics  
- Seasonality trends  
- Cancellation analysis  

---

## 6. SQL Transformations

### 6.1 CTE Usage
- Cleaning  
- Deduplication  
- Enrichment  
- Splitting raw → relational tables  

### 6.2 Temp Tables
- Intermediate joins  
- Validation staging  
- SCD staging  

### 6.3 Window Functions
- `RANK()`  
- `ROW_NUMBER()`  
- `LAG()` / `LEAD()`  
- RFM scoring  

### 6.4 SCD Type 2
- Customer country changes  
- Product description changes  
- Price changes  

---

## 7. Spark Transformations

### 7.1 DataFrame API
- Joins  
- Filters  
- Aggregations  
- Window functions  

### 7.2 Spark SQL
- CTEs  
- Temp views  
- MERGE INTO  

### 7.3 Delta Lake
- OPTIMIZE  
- VACUUM  
- ZORDER  
- MERGE INTO (SCD)  

### 7.4 Debugging
- Explain plans  
- Skew detection  
- Broadcast joins  
- Repartitioning  

---

## 8. Validation & QA

### Pre-write
- Schema validation  
- Null checks  
- Cancellation logic  

### Post-write
- Quantity × UnitPrice = TotalSpent  
- Row counts  
- Duplicate detection  
- SCD correctness  

---

## 9. Airflow Pipeline

### DAG Tasks
1. `extract_raw`  
2. `load_bronze`  
3. `transform_silver`  
4. `validate_silver`  
5. `transform_gold`  
6. `publish_gold`  

### Features
- Retries  
- Logging  
- SLA monitoring  
- Idempotency  

---

## 10. Documentation Artifacts

- Architecture diagram  
- Data dictionary  
- Transformation spec  
- Validation spec  
- SCD spec  
- Airflow DAG spec  
- README.md  

---

## 11. Roadmap & Checklists

### Phase 1 — Environment Setup
- [x] Install Python
- [x] Install uv  
- [ ] Install PySpark  
- [ ] Install Delta Lake  
- [ ] Install Airflow  
- [x] Create project repo  
- [ ] Create `src/` directory  
- [ ] Create `notebooks/` directory  
- [ ] Create `docs/` directory  

### Phase 2 — Raw Ingestion (Bronze)
- [ ] Download dataset  
- [ ] Convert Excel → CSV/Parquet  
- [ ] Write ingestion script  
- [ ] Infer schema  
- [ ] Normalize timestamps  
- [ ] Handle cancellations  
- [ ] Write Bronze Delta tables  

### Phase 3 — Cleaning & Relational Modeling (Silver)
- [ ] Create CTE-based cleaning SQL  
- [ ] Create temp table transformations  
- [ ] Split raw → relational tables  
- [ ] Deduplicate  
- [ ] Validate Quantity × UnitPrice  
- [ ] Write Silver Delta tables  

### Phase 4 — SCD Type 2
- [ ] Identify SCD candidates  
- [ ] Build SCD staging tables  
- [ ] Implement MERGE INTO logic  
- [ ] Write SCD dimension tables  

### Phase 5 — Gold Layer Analytics
- [ ] RFM segmentation  
- [ ] Customer lifetime value  
- [ ] Product performance  
- [ ] Country-level analytics  
- [ ] Seasonality trends  
- [ ] Cancellation analysis  

### Phase 6 — Airflow Orchestration
- [ ] Create DAG  
- [ ] Add ingestion task  
- [ ] Add silver transformation task  
- [ ] Add validation task  
- [ ] Add gold transformation task  
- [ ] Add publishing task  
- [ ] Add retries + logging  

### Phase 7 — Documentation
- [ ] Architecture diagram  
- [ ] Data dictionary  
- [ ] Transformation spec  
- [ ] Validation spec  
- [ ] SCD spec  
- [ ] Airflow DAG spec  
- [ ] README.md  

---

## 12. AI-Agent Specification

### Directory Structure

```
/project
    /src
        bronze_ingestion.py
        silver_transform.py
        gold_transform.py
        scd.py
        airflow_dag.py
    /notebooks
        exploration.ipynb
        validation.ipynb
        debugging.ipynb
    /docs
        PROJECT_SPEC.md
        architecture.md
        data_dictionary.md
        transformations.md
        scd.md
        airflow.md
    /data
        raw/
        bronze/
        silver/
        gold/
```

### Agent Suggestions

- All transformations must follow `/docs/transformations.md`  
- All SQL must use CTE-first patterns  
- All Spark code must use DataFrame API unless SQL is explicitly required  
- All merges must follow `/docs/scd.md`  
- All DAG tasks must follow `/docs/airflow.md`  

---

## 13. Versioning

- **v0.1** — Initial project specification  

---