# 🏗️ Data Warehouse & Business Analytics Project

Welcome to the **Data Warehouse and Analytics Project** repository!  
This project demonstrates a comprehensive, end-to-end data warehousing and analytics solution — from building a multi-layered data warehouse to generating actionable business insights using SQL. Designed as a portfolio project, it highlights industry best practices in data engineering and analytics.

---

## 📐 Data Architecture

![Data Architecture](docs/Data%20Architecture%20Diagaram.png)

This project follows the **Medallion Architecture** with three distinct layers:

| Layer | Description |
|-------|-------------|
| 🥉 **Bronze** | Raw data ingested as-is from CSV source files (CRM & ERP systems) into SQL Server |
| 🥈 **Silver** | Cleansed, standardized, and normalized data — deduplicated and ready for integration |
| 🥇 **Gold** | Business-ready star schema with dimension and fact tables for analytics and reporting |

---

## 📁 Repository Structure

```
SQL--Data-Warehouse-Project/
│
├── datasets/                               # Raw source data files
│   ├── source_crm/
│   │   ├── cust_info.csv                   # CRM customer data
│   │   ├── prd_info.csv                    # CRM product data
│   │   └── sales_details.csv              # CRM sales transactions
│   └── source_erp/
│       ├── CUST_AZ12.csv                   # ERP customer demographics
│       ├── LOC_A101.csv                    # ERP customer locations
│       └── PX_CAT_G1V2.csv                # ERP product categories
│
├── docs/                                   # Architecture diagrams and documentation
│   ├── Data Architecture Diagaram.png      # Medallion architecture overview
│   ├── Data Architecture Diagaram.drawio.pdf
│   ├── Data Flow Diagram.drawio.png        # End-to-end data flow
│   ├── Data Model.png                      # Star schema / data model
│   ├── Integration Model Diagram.jpg       # Source system integration
│   ├── data_catalog.md                     # Column-level metadata for Gold tables
│   ├── data_layers.pdf                     # Layer descriptions
│   └── naming_conventions.md              # Naming standards for all objects
│
├── scripts/
│   ├── init_database.sql                   # Creates DataWarehouse DB + Bronze/Silver/Gold schemas
│   │
│   ├── bronze/
│   │   ├── ddl_bronze.sql                  # Creates Bronze layer tables
│   │   └── proc_load_bronze.sql            # Stored procedure: load CSV → Bronze
│   │
│   ├── silver/
│   │   ├── ddl_silver.sql                  # Creates Silver layer tables
│   │   └── proc_load_silver.sql            # Stored procedure: transform Bronze → Silver
│   │
│   ├── gold/
│   │   └── ddl_gold.sql                    # Creates Gold views (star schema)
│   │
│   ├── EDA/                                # Exploratory Data Analysis scripts
│   │   ├── 00_init_database.sql            # Sets up DataWarehouseAnalytics DB for EDA
│   │   ├── 01_database_exploration.sql     # Explore tables and schemas
│   │   ├── 02_dimensions_exploration.sql   # Explore dimension table values
│   │   ├── 03_date_range_exploration.sql   # Temporal boundary analysis
│   │   └── 04_measures_exploration.sql     # Key metrics exploration
│   │
│   └── Analytics/                          # Business analytics and reporting scripts
│       ├── 01_magnitude_analysis.sql       # Totals and averages by category/country
│       ├── 02_ranking_analysis.sql         # Top/bottom performers with window functions
│       ├── 03_change_over_time_analysis.sql# Monthly and yearly sales trends
│       ├── 04_cumulative_analysis.sql      # Running totals and moving averages
│       ├── 05_performance_analysis.sql     # Year-over-year product performance
│       ├── 06_data_segmentation.sql        # Customer and product segmentation
│       ├── 07_part_to_whole_analysis.sql   # Category contribution to total revenue
│       ├── 08_report_customers.sql         # Customer KPI report view
│       └── 09_report_products.sql          # Product KPI report view
│
├── tests/
│   ├── quality_check_silver.sql            # Data quality checks for Silver layer
│   └── quality_check_gold.sql              # Referential integrity checks for Gold layer
│
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- [SQL Server Express](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (free)
- [SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)

### Setup Instructions

**1. Clone this repository**
```bash
git clone https://github.com/faizan97-malek/SQL--Data-Warehouse-Project.git
```

**2. Place the datasets** from the `datasets/` folder into the following local paths:
```
C:\sql\dwh_project\datasets\source_crm\
C:\sql\dwh_project\datasets\source_erp\
```

**3. Run the scripts in order** inside SSMS:

| Step | Script | Description |
|------|--------|-------------|
| 1 | `scripts/init_database.sql` | Creates the `DataWarehouse` database and all three schemas |
| 2 | `scripts/bronze/ddl_bronze.sql` | Creates Bronze tables |
| 3 | `EXEC bronze.load_bronze` | Loads raw CSV data into Bronze |
| 4 | `scripts/silver/ddl_silver.sql` | Creates Silver tables |
| 5 | `EXEC silver.load_silver` | Transforms and loads Bronze → Silver |
| 6 | `scripts/gold/ddl_gold.sql` | Creates Gold views (star schema) |

**4. (Optional) Run EDA scripts** in `scripts/EDA/` to explore the data structure and validate ranges.

**5. Run analytics scripts** in `scripts/Analytics/` against the Gold layer for business insights.

---

## 📊 Data Model

The Gold layer is modeled as a **star schema** optimized for analytical queries:

![Data Model](docs/Data%20Model.png)

```
gold.fact_sales
    ├── → gold.dim_customers  (joined on customer_key)
    └── → gold.dim_products   (joined on product_key)
```

Full column-level documentation is available in [`docs/data_catalog.md`](docs/data_catalog.md).

---

## 📈 Analytics & Reporting

The analytics scripts cover a full range of business intelligence techniques:

| Script | Technique | Key SQL Used |
|--------|-----------|-------------|
| `01_magnitude_analysis` | Totals and averages by category, country, customer | `SUM()`, `AVG()`, `GROUP BY` |
| `02_ranking_analysis` | Top/bottom product and customer performance | `RANK()`, `DENSE_RANK()`, `TOP` |
| `03_change_over_time_analysis` | Monthly and yearly sales trends | `DATETRUNC()`, `FORMAT()`, `YEAR()` |
| `04_cumulative_analysis` | Running totals and moving averages | `SUM() OVER()`, `AVG() OVER()` |
| `05_performance_analysis` | Year-over-year product comparisons | `LAG()`, `CASE`, `PARTITION BY` |
| `06_data_segmentation` | Customer (VIP/Regular/New) and product cost tiers | `CASE`, CTEs |
| `07_part_to_whole_analysis` | Category revenue contribution percentages | `SUM() OVER()`, `CAST()` |
| `08_report_customers` | Reusable customer KPI view | Recency, AOV, avg monthly spend |
| `09_report_products` | Reusable product KPI view | Recency, AOR, avg monthly revenue |

---

## 🧪 Data Quality Testing

Quality checks are included for both the Silver and Gold layers:

- **Silver** (`tests/quality_check_silver.sql`) — checks for NULL/duplicate PKs, unwanted whitespace, invalid date ranges, and sales calculation consistency
- **Gold** (`tests/quality_check_gold.sql`) — checks surrogate key uniqueness and referential integrity between `fact_sales` and both dimension tables

Run these after each layer is loaded to validate data integrity before proceeding.

---

## 🛠️ Tech Stack

- **Database:** Microsoft SQL Server
- **Language:** T-SQL
- **ETL:** Stored procedures with `TRY/CATCH` error handling and load duration logging
- **Diagramming:** Draw.io
- **Project Planning:** [Notion](https://www.notion.so/DATA-WAREHOUSE-PROJECT-35ba4da43fcc80f182e0dfcf30932bd0)

---

## 📄 License

This project is licensed under the MIT License. See [`LICENSE`](LICENSE) for details.

---

## 👨‍💻 About Me

Hi, I'm **Faizan Farid** — a data analytics professional with a background in education and a passion for turning raw data into meaningful insights.

I hold a **Master of Science in Mathematics** and a **Bachelor of Education**, and I'm currently completing a **Master of Data Analytics** at the University of Niagara Falls Canada. My journey from teaching mathematics and science to data engineering has given me a unique ability to break down complex technical concepts and communicate them clearly.

This project reflects my hands-on approach to learning — building a production-style data warehouse from scratch using industry best practices in ETL pipeline development, data modeling, and SQL-based analytics.

I'm actively seeking opportunities in **data analytics** where I can contribute both technical skills and a structured, problem-solving mindset.
