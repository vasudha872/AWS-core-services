# AWS Core Services Hands-On Lab: E-Commerce Sales Data Analytics

## 🎯 Objective
This project demonstrates the implementation of a serverless data analytics pipeline using AWS core services (S3, Glue, CloudWatch, and Athena) to analyze e-commerce sales data.

## 📊 Dataset
- **Source:** [Kaggle E-Commerce Sales Dataset](https://www.kaggle.com/datasets/thedevastator/unlock-profits-with-e-commerce-sales-data)
- **Format:** CSV
- **Records:** ~128,000 Amazon sale transactions
- **Columns:** 24 attributes including order details, shipping info, product data, and transaction amounts

## 🏗️ Architecture Overview

### AWS Services Used:
1. **Amazon S3:** Object storage for raw data and query results
2. **AWS Glue:** Data catalog and schema discovery via Crawler
3. **Amazon CloudWatch:** Monitoring and logging for Glue Crawler
4. **Amazon Athena:** Serverless SQL query engine
5. **AWS IAM:** Security and access management

### Architecture Flow:
```
CSV Data → S3 (Raw Bucket) → Glue Crawler → Data Catalog → Athena → S3 (Processed Bucket)
                                    ↓
                              CloudWatch Logs
```

## 🛠️ Implementation Steps

### 1. S3 Bucket Configuration
- **Raw Data Bucket:** `ecommerce-raw-bucket-vm-9876`
  - Purpose: Store original CSV file
  - Location: Root directory
  
- **Processed Data Bucket:** `ecommerce-processed-bucket-vm-9876`
  - Purpose: Store Athena query results
  - Location: `/athena-results/` folder

### 2. IAM Role Setup
- **Role Name:** `GlueCrawlerRole`
- **Trusted Entity:** AWS Glue
- **Permissions:**
  - `AWSGlueServiceRole` - Glue operations
  - `AmazonS3FullAccess` - S3 read/write access

### 3. Glue Configuration
- **Database:** `ecommerce_database`
- **Crawler:** `ecommerce_crawler`
  - Data Source: S3 raw bucket
  - Output: Automated table creation with schema inference
  - Schedule: On-demand
  
- **Table Created:** `raw_ecommerce_raw_bucket_vm_9876`
  - Columns: 24 fields
  - Format: CSV with header row
  - SerDe: OpenCSV

### 4. CloudWatch Monitoring
- Successfully monitored Glue Crawler execution
- Verified logs showing:
  - Crawler initialization
  - Schema classification
  - Catalog update
  - Successful completion

### 5. Athena Query Execution
- Query result location configured: `s3://ecommerce-processed-bucket-vm-9876/athena-results/`
- Database: `ecommerce_database`
- Table: `raw_ecommerce_raw_bucket_vm_9876`

## 📈 Analytical Queries

### Query 1: Cumulative Sales Over Time
**Purpose:** Calculate running total of sales for 2022  
**Technique:** Window function with ROWS BETWEEN  
**Business Value:** Track revenue accumulation and cash flow trends

### Query 2: Geographic Hotspot Analysis
**Purpose:** Identify states with problematic orders (cancelled)  
**Technique:** Filtering and grouping  
**Business Value:** Pinpoint regions needing operational improvements

### Query 3: Fulfillment Impact Analysis
**Purpose:** Compare Amazon vs Merchant fulfillment performance by category  
**Technique:** Aggregation with window functions for percentages  
**Business Value:** Optimize fulfillment strategy by category

### Query 4: Top 3 Products Per Category
**Purpose:** Rank best-selling products within each category  
**Technique:** CTE with ROW_NUMBER() window function and PARTITION BY  
**Business Value:** Focus marketing efforts on top performers

### Query 5: Monthly Growth Analysis
**Purpose:** Calculate month-over-month growth rates for orders and sales  
**Technique:** LAG() window function for time-series analysis  
**Business Value:** Measure business momentum and forecast trends



## 🚀 How to Reproduce

1. **Prerequisites:**
   - AWS Account with appropriate permissions
   - Dataset downloaded from Kaggle

2. **Setup Steps:**
```bash
   # Create S3 buckets
   aws s3 mb s3://ecommerce-raw-bucket-[your-id]
   aws s3 mb s3://ecommerce-processed-bucket-[your-id]
   
   # Upload data
   aws s3 cp amazon_sales_data.csv s3://ecommerce-raw-bucket-[your-id]/
```

3. **Follow the implementation steps** outlined in this README

4. **Run queries** from `handson11_sql.txt` in Athena




### **Screenshots**
![WhatsApp Image 2025-10-28 at 21 42 20_07af65ef](https://github.com/user-attachments/assets/3506faf8-03c3-490e-a2f7-5da881d18faf)
![WhatsApp Image 2025-10-28 at 22 20 41_00c8ff86](https://github.com/user-attachments/assets/34132cc7-a18c-4cf1-8cad-505383c12156)
![WhatsApp Image 2025-10-28 at 22 21 22_b2e43579](https://github.com/user-attachments/assets/f51a4e72-26ad-43e2-ba1f-978ed840ae33)
![WhatsApp Image 2025-10-28 at 22 23 04_e6e1f8f9](https://github.com/user-attachments/assets/96ee34a4-b0e0-49cb-a413-58f9a567db04)



