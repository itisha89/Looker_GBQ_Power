# BigQuery Data Analysis and Visualization Project

#### Project Overview
This project focuses on leveraging Google BigQuery to analyze a dataset named `datasets.power`, which contains operational metrics of network devices, and visualizing the results using Looker Studio. The project demonstrates essential steps such as data exploration, transformation, and summarization, culminating in creating a dynamic and insightful dashboard.

---

### Objectives
1. **Fetch and Explore Data:**
   - Perform queries to understand the structure and volume of data.
   - Retrieve summary statistics and analyze distinct values across key columns.

2. **Data Cleaning and Transformation:**
   - Drop unnecessary columns to reduce data size and improve query performance.
   - Parse and transform the `__time` column into structured components like `day`, `month`, `date`, and `year`.
   - Add and modify columns to enhance usability for analysis.

3. **Data Visualization:**
   - Create a smaller, cleaned dataset (`power_small`) in BigQuery.
   - Use Looker Studio to build an interactive dashboard for data analysis and insights.

---

### Dataset Description
The dataset `datasets.power` contains the following columns:
- `__time`: Timestamp of the data record.
- `friendlyHostname`: Hostname of the device.
- `location`: Location of the device.
- `ipAddress`: IP address of the device.
- `sneId`: Device ID.
- `voltageAvg`, `voltageSum`: Voltage metrics.
- `wattageAvg`, `wattageSum`: Power consumption metrics.
- Additional metadata such as `platform`, `type`, `vendor`, `model`, `interfaceName`, and `hostname`.

---

### Steps to Implement

#### 1. Data Exploration
The following queries were executed to understand the dataset:
- **Count Rows:**
  ```sql
  SELECT COUNT(*) AS num_rows FROM datasets.power;
  ```
- **Inspect Schema:**
  ```sql
  SELECT column_name, data_type 
  FROM `datasets.INFORMATION_SCHEMA.COLUMNS` 
  WHERE table_name = 'power';
  ```
- **Preview Data:**
  ```sql
  SELECT * FROM datasets.power LIMIT 10;
  ```

#### 2. Summarize Distinct Values
- Gathered distinct counts for various columns to evaluate their uniqueness and relevance:
  ```sql
  SELECT 
    COUNT(DISTINCT friendlyHostname) AS num_f_hname, 
    COUNT(DISTINCT hostname) AS num_hname, 
    COUNT(DISTINCT interfaceName) AS num_interfacename,
    COUNT(DISTINCT ipAddress) AS num_ipAddress,
    COUNT(DISTINCT location) AS num_location,
    COUNT(DISTINCT model) AS num_model,
    COUNT(DISTINCT platform) AS num_platform,
    COUNT(DISTINCT sneId) AS num_sneId,
    COUNT(DISTINCT type) AS num_type,
    COUNT(DISTINCT vendor) AS num_vendor,
    COUNT(DISTINCT voltageAvg) AS num_voltageAvg,
    COUNT(DISTINCT wattageAvg) AS num_wattageAvg,
    COUNT(DISTINCT voltageSum) AS num_voltageSum,
    COUNT(DISTINCT wattageSum) AS num_wattageSum 
  FROM datasets.power;
  ```

#### 3. Data Cleaning
- Removed less significant columns to create a streamlined dataset:
  ```sql
  CREATE TABLE datasets.power_small AS
  SELECT * EXCEPT(interfaceName, model, platform, type, vendor, hostname)
  FROM datasets.power;
  ```

- Extracted structured date components:
  ```sql
  CREATE OR REPLACE TABLE datasets.power_small AS
  SELECT *, 
    REGEXP_EXTRACT(__time, r'([A-Za-z]{3})') AS day,
    REGEXP_EXTRACT(__time, r'(\s[A-Za-z]{3}\s)') AS month,
    REGEXP_EXTRACT(__time, r'(\d{2})') AS date,
    REGEXP_EXTRACT(__time, r'(\d{4})') AS year
  FROM datasets.power_small;
  ```

#### 4. Data Transformation
- Added columns with correct data types:
  ```sql
  ALTER TABLE datasets.power_small ADD COLUMN date_new1 INT64;
  UPDATE datasets.power_small 
  SET date_new1 = CAST(date AS INT64) WHERE date IS NOT NULL;
  ```

#### 5. Validation
- Verified the transformations:
  ```sql
  SELECT * FROM `datasets.power_small`;
  ```

---

### Data Visualization
- **Tool:** Looker Studio
- **Objective:** Develop an interactive dashboard to visualize metrics such as voltage and wattage trends, device locations, and more.
- **Key Metrics:** 
  - Average Voltage (`voltageAvg`)
  - Average Wattage (`wattageAvg`)
  - Distribution by `location` and `friendlyHostname` and `iPAddress`.

---

### Key SQL Highlights
- Extracted and transformed date-time components to facilitate temporal analysis.
- Optimized the dataset by removing redundant columns, reducing storage and improving query performance.

---

### Dashboard Insights
The dashboard created in Looker Studio provides:
- Real-time monitoring of power and voltage metrics.
- Device-wise and location-wise comparisons.
- Aggregated trends for better decision-making.

---

### Prerequisites
- Access to Google BigQuery and Looker Studio.
- Basic understanding of SQL and data visualization principles.


## 🌍 Explore More Projects  
For more exciting machine learning and AI projects, visit **[The iVision](https://theivision.wordpress.com/)** and stay updated with the latest innovations and research! 🚀  

---
