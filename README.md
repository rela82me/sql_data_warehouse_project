# SQL Data Warehouse Project

## Overview
This project implements a robust **SQL Data Warehouse** using a **Medallion Architecture** (Bronze, Silver, Gold). It demonstrates a complete ETL (Extract, Transform, Load) pipeline, transforming raw CSV data into clean, structured, and business-ready dimensional models for analytics.

## Project Architecture
The data warehouse is organized into three distinct layers:
- **🥉 Bronze (Raw):** Landing zone for raw data. Tables mirror the source CSV files exactly, preserving the original state without any transformations.
- **🥈 Silver (Cleaned):** Data cleaning and standardization layer. This stage handles data typing, trimming, null handling, deduplication, and standardizing gender/marital status codes.
- **🥇 Gold (Curated):** Business layer containing Dimensional Models (Star Schema). It consists of views like `dim_customer`, `dim_products`, and `fact_sales` that are optimized for reporting and BI tools.

## Repository Structure
```text
├── datasets/             # Source CSV files (CRM & ERP data)
├── docs/                 # Project documentation and diagrams
├── scripts/
│   ├── Create init_database.sql   # Database and Schema initialization
│   ├── bronze/           # DDL and Load scripts for the Bronze layer
│   ├── silver/           # DDL and Load scripts for the Silver layer
│   └── gold/             # DDL and View definitions for the Gold layer
└── tests/                # Data validation and quality check scripts
```

## Technology Stack
- **Database Engine:** Microsoft SQL Server
- **Language:** T-SQL (Transact-SQL)
- **Tools:** SQL Server Management Studio (SSMS) / Azure Data Studio

## Setup & Installation

### 1. Initialize Database
Run the [init_database.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/Create%20init_database.sql) script to create the `DataWarehouse` database and the `bronze`, `silver`, and `gold` schemas.
> [!WARNING]
> This script will drop the database if it already exists.

### 2. Configure Data Paths
Before loading data, update the file paths in [bronze_load.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/bronze/bronze_load.sql) to point to your local `datasets` directory.
```sql
BULK INSERT bronze.crm_cust_info
FROM 'C:\Your\Path\To\sql_data_warehouse_project\datasets\cust_info.csv'
-- ...
```

### 3. Build the Warehouse
Execute the scripts in the following order:
1. **Bronze DDL:** [ddl_bronze.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/bronze/ddl_bronze.sql)
2. **Bronze Load:** `EXEC bronze.load_bronze` (from [bronze_load.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/bronze/bronze_load.sql))
3. **Silver DDL:** [ddl_silver.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/Silver/ddl_silver.sql)
4. **Silver Load:** `EXEC silver.load_silver` (from [silver_load.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/Silver/silver_load.sql))
5. **Gold DDL:** [ddl_gold.sql](file:///c:/Users/rela8/Documents/C%20VS%20Code/Project%20Files/SQL/sql_data_warehouse_project/scripts/gold/ddl_gold.sql)

## Data Transformation Highlights
- **Deduplication:** Using `ROW_NUMBER()` in the Silver layer to keep only the latest records.
- **Data Cleaning:** Handling malformed dates, negative sales values, and inconsistent gender labels.
- **Dimensional Modeling:** Creating a Star Schema in the Gold layer with surrogate keys for optimized querying.

---
*Created by [rela82me](https://github.com/rela82me)*
