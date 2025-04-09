# Chicago Taxi & Weather Data Pipeline 🚕

This project implements an automated daily ETL pipeline that processes historical data (2 months prior to the current date) for both Chicago taxi trips and weather. The data is pulled, transformed, enriched with master data, and uploaded to Amazon S3 for further use.

---

## 📆 Data Scope

- Processes data from **2 months prior** to the current date (rolling window)
- Designed to run **daily**, incrementally

---

## 🔁 Pipeline Steps

1. **Fetch data from S3**
   - Download raw taxi and weather data files for the specific target date from the source S3 bucket.

2. **Transform weather data**
   - Clean and format the weather dataset
   - Standardize columns, handle missing values, and ensure consistent timestamp formats

3. **Transform taxi trips data** 
   - Clean and normalize trip records
   - Format datetime, drop nulls, convert types, etc.

4. **Update `payment_type_master` table** 
   - Extract unique `payment_type` values
   - Update the master table with any new types (ID + Name mapping)

5. **Update `company_master` table** 
   - Extract unique `company` values
   - Update the company master table with any new entries

6. **Enrich `taxi_trips` with master data** 
   - Replace `company` and `payment_type` string values in taxi trip records
   - Use corresponding IDs from the latest master tables

7. **Upload transformed weather data to S3**
   - Save and push cleaned weather data to the processed-data S3 bucket

8. **Upload enriched taxi trips data to S3**
   - Save and upload the final taxi trip data with foreign keys

9. **Upload updated master tables**
   - Save the latest `payment_type_master` and `company_master` tables to S3

---

## ☁️ Buckets Used

- `raw-data-bucket` – source of the original files
- `processed-data-bucket` – destination for transformed and enriched datasets

---

## 🧰 Tools & Technologies

- Python
- Pandas
- boto3 (AWS)
- S3
- Scheduled with Lambda

---

