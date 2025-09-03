# PySpark New York Taxi Data Analysis

This project uses **PySpark** on a **Google Cloud Platform (GCP)** cluster with **Dataproc** to analyze a large dataset of New York City taxi trips. The analysis is performed within a Jupyter notebook, which allows for an interactive and reproducible workflow.

-----

## ⚙️ Technologies Used

  * **PySpark**: A Python API for Apache Spark, used for distributed data processing.
  * **Google Cloud Dataproc**: A managed service for running Apache Spark and Hadoop clusters, simplifying cluster management.
  * **Jupyter Notebook**: An interactive computing environment for writing and running code.
  * **Google Cloud Storage (GCS)**: Used to store the Parquet-formatted taxi trip data.

-----

## 📊 Project Steps

The analysis follows a series of steps to load, transform, and query the data.

### 1\. Data Loading 💾

The project begins by creating a Spark session and loading the taxi trip data from a Parquet file stored in a Google Cloud Storage bucket.

```python
# Create a Spark session
spark = SparkSession.builder.appName("MCD-G1").getOrCreate()

# Read the data from GCS
df = spark.read.parquet("gs://mcd_procesamiento_disrtibuido/zone=landing/src=new_yorktaxi")
```

### 2\. Data Exploration 🕵️

Before analysis, the data's structure and size are examined.

  * **Schema**: The schema is printed to understand the columns and their data types, such as `timestamp_ntz` for dates and `decimal(38,9)` for numerical values.
  * **Record Count**: The total number of records is counted, revealing a massive dataset of over 102 million taxi trips.

<!-- end list -->

```python
# Print the schema
df.printSchema()

# Check the amount of records
df.count()
```

### 3\. Feature Engineering 🛠️

New columns are created to enrich the dataset for further analysis.

  * **Trip Duration**: A `trip_duration_minutes` column is added by calculating the difference between the pickup and dropoff timestamps and rounding it to the nearest minute.
  * **Payment Type Description**: A human-readable `payment_type_desc` column is created by mapping numerical `payment_type` codes to descriptions like 'Credit Card' and 'Cash'.

### 4\. Key Analyses 📈

Several analytical queries are run to extract insights from the data.

  * **Trips per Vendor**: The number of trips is aggregated by vendor ID, year, and month to see trip volume distribution over time.
  * **Average Trip Cost**: The overall average `total_amount` for all trips is calculated. The result is **$16.43**.
  * **Average Trip Cost by Payment Type**: The average `total_amount` is also calculated, but this time, it's grouped by the `payment_type_desc` to compare average fares across different payment methods.
