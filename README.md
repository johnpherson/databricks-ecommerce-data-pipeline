# Retail Data Pipeline - Bronze, Silver, Gold Architecture

A comprehensive data engineering pipeline demonstrating the modern **Medallion Architecture** (Bronze-Silver-Gold) for retail analytics using Apache Spark and Delta Lake.

## 🏗️ Architecture Overview

This project implements a complete data pipeline following the **Medallion Architecture** pattern:

```
📥 BRONZE (Raw Data)    →    🔧 SILVER (Clean Data)    →    🏆 GOLD (Business Metrics)
   ├─ Raw ingestion           ├─ Data quality              ├─ Monthly metrics
   ├─ Metadata tracking       ├─ Transformations           ├─ Customer segmentation  
   └─ Schema evolution        ├─ Business logic            ├─ Product performance
                              └─ Data enrichment           └─ Geographic analysis
```

## 🎯 Project Objectives

- **Demonstrate modern data engineering practices** using industry-standard tools
- **Implement data quality controls** and validation processes
- **Create business-ready analytics tables** for downstream consumption
- **Show end-to-end pipeline** from raw data to executive dashboards
- **Practice with real-world retail data** including customer transactions

## 🛠️ Technologies Used

| Technology | Purpose | Version |
|------------|---------|---------|
| **Apache Spark** | Distributed data processing | 3.x |
| **Delta Lake** | ACID transactions & versioning | 2.x |
| **Databricks** | Cloud analytics platform | Runtime 13.x |
| **Python** | Pipeline development | 3.9+ |
| **PySpark** | Spark Python API | 3.x |

## 📊 Dataset

**Source:** UCI Machine Learning Repository - Online Retail Dataset
- **Records:** ~500K transactions
- **Time Period:** 2010-2011
- **Geography:** UK-based online retailer
- **Scope:** Customer transactions, product details, geographic data

**Key Fields:**
- `InvoiceNo` - Transaction identifier
- `StockCode` - Product identifier  
- `Description` - Product description
- `Quantity` - Items purchased
- `InvoiceDate` - Transaction timestamp
- `UnitPrice` - Product price
- `CustomerID` - Customer identifier
- `Country` - Customer location

## 🔄 Pipeline Stages

### 1. 📥 Bronze Layer (Raw Data Ingestion)
**Purpose:** Ingest raw data with minimal processing

**Features:**
- Raw data ingestion from UCI repository
- Metadata tracking (ingestion timestamp, batch ID, source system)
- Schema evolution support
- Data lineage tracking

**Output:** `bronze_raw_transactions` table

### 2. 🔧 Silver Layer (Data Quality & Transformation)
**Purpose:** Clean, validate, and enrich data

**Data Quality Rules:**
- Remove records with null `CustomerID`
- Filter out negative quantities
- Remove zero or negative prices
- Validate required fields

**Transformations:**
- Parse dates and add time dimensions
- Calculate revenue (`Quantity × UnitPrice`)
- Add business categorizations
- Seasonal classifications

**Output:** `silver_clean_transactions` table

### 3. 🏆 Gold Layer (Business Metrics)
**Purpose:** Create business-ready aggregated tables

**Business Tables Created:**
- **Monthly Metrics** - Revenue, customers, transactions by month
- **Customer Segmentation** - VIP, High Value, Medium, Low Value customers
- **Product Performance** - Top sellers, revenue by product
- **Geographic Analysis** - Performance by country/region

**Output:** Multiple gold tables for analytics consumption

## 📈 Key Business Metrics

### Customer Analytics
- **Customer Lifetime Value (CLV)** 
- **Customer Segmentation** (VIP, High, Medium, Low Value)
- **Purchase Behavior** (Frequency, Recency, Monetary)
- **Geographic Distribution**

### Product Analytics  
- **Revenue by Product** 
- **Product Performance Tiers**
- **Sales Velocity**
- **Product Popularity Scores**

### Financial Metrics
- **Monthly/Quarterly Revenue**
- **Average Order Value**
- **Revenue per Customer**
- **Seasonal Trends**

## 🚀 Getting Started

### Prerequisites
- Databricks workspace access (Community Edition works)
- Python 3.9+
- Apache Spark 3.x

### Installation & Setup

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/retail-data-pipeline.git
cd retail-data-pipeline
```

2. **Set up Databricks environment**
```python
# Install required packages in Databricks
%pip install ucimlrepo
```

3. **Configure pipeline parameters**
```python
CATALOG_NAME = "your_catalog"
SCHEMA_NAME = "retail_pipeline"
```

### Running the Pipeline

**Option 1: Full Pipeline (Databricks)**
```python
# Run all cells in sequence
# 1. Bronze layer ingestion
bronze_count = ingest_raw_data()

# 2. Silver layer transformation  
silver_count = create_silver_layer()

# 3. Gold layer aggregations
gold_tables = create_gold_layer()
```

**Option 2: Jupyter Notebook (Local)**
```bash
# Install dependencies
pip install pyspark delta-spark ucimlrepo

# Run the notebook
jupyter notebook retail_pipeline.ipynb
```

## 📊 Usage Examples

### Query Monthly Revenue Trends
```sql
SELECT Year, Month, season, total_revenue, unique_customers
FROM gold_monthly_metrics
ORDER BY Year, Month;
```

### Find VIP Customers
```sql
SELECT CustomerID, Country, total_spent, customer_segment
FROM gold_customer_segments  
WHERE customer_segment = 'VIP'
ORDER BY total_spent DESC;
```

### Top Performing Products
```sql
SELECT StockCode, Description, total_revenue, unique_buyers
FROM gold_product_metrics
ORDER BY total_revenue DESC
LIMIT 10;
```

## 🔍 Data Quality Monitoring

The pipeline includes comprehensive data quality monitoring:

- **Completeness Checks** - Null value detection
- **Validity Checks** - Data type and range validation
- **Consistency Checks** - Business rule validation
- **Uniqueness Checks** - Duplicate detection
- **Timeliness Checks** - Data freshness validation

**Quality Metrics Tracked:**
- Data quality pass rate
- Records filtered by quality rules
- Schema evolution events
- Processing performance metrics

## 📁 Project Structure

```
retail-data-pipeline/
├── notebooks/
│   ├── 01_bronze_ingestion.py
│   ├── 02_silver_transformation.py
│   ├── 03_gold_aggregation.py
│   └── 04_data_quality_report.py
├── src/
│   ├── ingestion/
│   ├── transformation/
│   └── aggregation/
├── config/
│   └── pipeline_config.py
├── tests/
│   └── test_pipeline.py
├── docs/
│   └── architecture_diagram.png
└── README.md
```

## 🎨 Business Intelligence Integration

The Gold layer tables are designed for direct consumption by:

- **Tableau** - Connect to Delta tables
- **Power BI** - Use Databricks connector
- **Looker** - Query gold tables directly
- **Custom Dashboards** - REST API access

**Pre-built Views:**
- `executive_dashboard` - High-level KPIs
- `vip_customers` - Customer retention focus
- `top_products` - Product performance analysis

## 🚀 Scaling & Production Considerations

### Performance Optimization
- **Partitioning** by date for time-series data
- **Z-ordering** for faster queries
- **Caching** frequently accessed tables
- **Compact operations** for Delta Lake optimization

### Monitoring & Alerting
- Data quality threshold alerts
- Pipeline failure notifications
- Performance degradation detection
- Schema change notifications

### Security & Governance
- Column-level access controls
- Data masking for sensitive fields
- Audit logging for compliance
- Data lineage tracking

## 🤝 Contributing

1. **Fork the repository**
2. **Create feature branch** (`git checkout -b feature/amazing-feature`)
3. **Commit changes** (`git commit -m 'Add amazing feature'`)
4. **Push to branch** (`git push origin feature/amazing-feature`)
5. **Open Pull Request**

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **UCI Machine Learning Repository** for the retail dataset
- **Databricks** for the cloud analytics platform
- **Delta Lake** community for the open-source storage layer
- **Apache Spark** community for the distributed computing framework

---

## 🔗 Links & Resources

- **Dataset Source:** [UCI ML Repository](https://archive.ics.uci.edu/dataset/352/online+retail)
- **Databricks Documentation:** [docs.databricks.com](https://docs.databricks.com)
- **Delta Lake Guide:** [delta.io](https://delta.io)
- **Medallion Architecture:** [Databricks Blog](https://databricks.com/glossary/medallion-architecture)

---

**Built with ❤️ for the data engineering community**

*For questions, issues, or contributions, please open an issue or reach out!*
