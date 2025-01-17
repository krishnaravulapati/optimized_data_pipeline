
# Optimized Data Pipelines with Apache Spark on EMR Serverless

This repository demonstrates a medium-level use case of building an optimized data pipeline to process **e-commerce sales data** using **Apache Spark** on **Amazon EMR Serverless**. The pipeline is designed to ingest raw data from Amazon S3, perform data cleaning and transformations, generate aggregate reports, and load the processed data into **Amazon Redshift** for analytics.

---

## 🛠️ **Use Case Overview**

### Objective
- Process raw e-commerce sales data stored in Amazon S3.
- Perform data cleaning, transformation, and aggregation using Apache Spark.
- Store processed data in Amazon Redshift for analytics.

### Workflow
1. **Raw Data**:
   - Stored in Amazon S3 as CSV files.
   - Sample schema: `order_id, product_id, category, quantity, price, order_date`.

2. **Processing**:
   - Utilize Apache Spark on EMR Serverless to:
     - Cleanse data (e.g., handle missing values).
     - Compute total sales and other aggregations.
     - Write the processed data back to S3 in Parquet format.

3. **Analytics**:
   - Load the processed data into Amazon Redshift for further analysis.

---

## 🗂️ **Folder Structure**

```
.
├── scripts/
│   ├── process_sales_data.py     # PySpark script for data processing
│   ├── load_to_redshift.py       # Script to load data into Amazon Redshift
├── data/
│   ├── raw/                      # Raw sales data (CSV format)
│   ├── processed/                # Processed data (Parquet format)
└── README.md                     # Project documentation
```

---

## 📋 **Sample Data**

### Raw Data (`sales_data.csv`)
| order_id | product_id | category      | quantity | price  | order_date  |
|----------|------------|---------------|----------|--------|-------------|
| 1        | 101        | Electronics   | 2        | 300.00 | 2025-01-15  |
| 2        | 102        | Fashion       | 1        | 50.00  | 2025-01-15  |
| 3        | 103        | Home          | 4        | 25.00  | 2025-01-15  |
| 4        |            | Electronics   | 1        | 150.00 | 2025-01-15  |
| 5        | 104        | Fashion       | 2        |        | 2025-01-15  |

---

## 🖥️ **Code Breakdown**

### 1. `process_sales_data.py` (PySpark Script)
The script processes raw data from S3 using Apache Spark:
- Cleans missing or invalid entries.
- Calculates total sales (`quantity * price`) for each transaction.
- Aggregates total and average sales by category.
- Writes the output to S3 in Parquet format.

#### Example Output (`sales_data.parquet`):
| category      | total_sales | avg_sales |
|---------------|-------------|-----------|
| Electronics   | 750.00      | 375.00    |
| Fashion       | 100.00      | 50.00     |
| Home          | 100.00      | 100.00    |

---

### 2. `load_to_redshift.py` (Python Script)
This script uses **boto3** and **psycopg2** to load the processed Parquet data from S3 into Amazon Redshift.

---

## ⚙️ **How to Run**

### Prerequisites
1. AWS CLI installed and configured.
2. Amazon EMR Serverless application set up.
3. Amazon Redshift cluster available.

### Steps
1. **Upload the scripts to S3**:
   ```bash
   aws s3 cp process_sales_data.py s3://your-bucket-name/scripts/
   ```
2. **Run the EMR Serverless job**:
   ```bash
   aws emr-serverless start-job-run        --application-id <application-id>        --execution-role-arn <execution-role-arn>        --job-driver '{
           "sparkSubmit": {
               "entryPoint": "s3://your-bucket-name/scripts/process_sales_data.py",
               "sparkSubmitParameters": "--conf spark.executor.memory=2g --conf spark.executor.cores=2"
           }
       }'        --configuration-overrides '{
           "monitoringConfiguration": {
               "s3MonitoringConfiguration": {
                   "logUri": "s3://your-bucket-name/logs/"
               }
           }
       }'
   ```
3. **Load data into Redshift**:
   ```bash
   python load_to_redshift.py
   ```

---

## 📊 **Expected Outcome**
- **Cleaned and Processed Data**: Available in Amazon S3 as Parquet files.
- **Analytics-Ready Data**: Available in Amazon Redshift for BI tools like Tableau or AWS QuickSight.

---

## 🌟 **Features**
- Serverless processing with Amazon EMR for cost efficiency.
- Scalable and reliable data pipeline.
- Analytics-ready output for business intelligence.

---

## 📧 **Contact**
If you have any questions or suggestions, feel free to reach out:
- **Email**: kanthravulapati@gmail.com
- **GitHub**: [https://github.com/krishnaravulapati](https://github.com/krishnaravulapati)

---
