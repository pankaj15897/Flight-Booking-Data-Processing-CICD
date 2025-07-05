# Flight-Booking-Data-Processing-CICD


This project demonstrates a fully-managed, end-to-end **ETL pipeline on Google Cloud Platform** using services like **Cloud Composer (Airflow)**, **Dataproc (PySpark)**, **BigQuery**, and **Cloud Storage**.  
It processes raw flight booking data and transforms it into analytical insights in a scalable and serverless manner.

---

## 📌 Project Overview

- Triggered automatically when a **CSV file is uploaded to a GCS bucket**
- Orchestrated using **Cloud Composer** (Apache Airflow)
- Runs a **PySpark job on Dataproc Serverless** to perform data transformation
- Loads the cleaned and enriched data into **BigQuery** for analysis and reporting

---

## 🧰 Tech Stack

| Tool/Service         | Purpose                                |
|----------------------|-----------------------------------------|
| **Google Cloud Storage (GCS)** | Raw data source + intermediate storage      |
| **Cloud Composer (Airflow)**   | DAG orchestration and monitoring           |
| **Dataproc Serverless**        | Executes PySpark transformation logic      |
| **PySpark**                    | Data processing and enrichment             |
| **BigQuery**                  | Destination for analytical output          |
| **Python**                    | Glue logic and custom operators (optional) |

---

## 🗂️ Pipeline Architecture

```text
           +-------------+
           |  CSV Upload |
           |  to GCS     |
           +------+------+
                  |
         Trigger via Airflow (Cloud Composer)
                  |
           +------+------+
           |  Dataproc   |  ← PySpark transformation:
           |  Serverless |     - Clean raw data
           +------+------+
                  |
         Load to BigQuery (staging/prod)
                  |
           +------+------+
           | Analytics   |
           | / Dashboards|
           +-------------+
````

---

## 🧪 PySpark Transformations

Located in `spark_transformation_job.py`:

* ✅ Null handling, column selection
* ✅ Feature engineering:

  * `booking_lead_time` (in days)
  * Lead time categorization (`short`, `medium`, `long`)
  * Route string: `origin -> destination`
* ✅ Filtering for only confirmed bookings
* ✅ Output written in Parquet format to GCS and loaded into BigQuery

---

## ⚙️ Airflow DAG

Located in `airflow_job.py`:

* Watches a GCS bucket for new files
* Submits the Dataproc job using `DataprocSubmitJobOperator`
* Supports staging and production environments via Airflow variables

---

## 📝 How to Run This Project

### 1. Set Up GCP Environment

* Create a GCS bucket (e.g., `gs://your-bucket-name/data/raw/`)
* Create a BigQuery dataset (e.g., `flight_data`)
* Enable APIs: Composer, Dataproc, BigQuery, Storage

### 2. Deploy Composer DAG

* Upload `airflow_job.py` to your Composer environment’s `/dags/` folder
* Set required Airflow Variables:

  * `environment` = `dev` or `prod`
  * `pyspark_file_path` = GCS path to `spark_transformation_job.py`
  * `output_table` = BigQuery destination table name

### 3. Upload Sample Data

* Drop a raw CSV file into `gs://<your-bucket>/data/raw/`
* DAG will trigger automatically

---

## 📊 Example Output in BigQuery

| booking\_id | route     | lead\_time\_days | category | amount |
| ----------- | --------- | ---------------- | -------- | ------ |
| BK001       | DEL → BOM | 2                | short    | 4500   |
| BK002       | BLR → HYD | 10               | medium   | 5800   |
| BK003       | BOM → NYC | 40               | long     | 72000  |

---

## 🚀 Future Enhancements

* ✅ Integrate Data Quality Checks (e.g., Great Expectations)
* 📈 Add dashboard layer with Looker Studio or Data Studio
* 🔒 Apply role-based access using IAM for staging/prod
* 🌍 Add geolocation enrichment based on airport codes

---

## 🧠 Learning Outcomes

* Built a scalable GCP-native data pipeline with **serverless architecture**
* Hands-on with **Airflow DAGs**, **PySpark on Dataproc**, **GCS triggers**, and **BigQuery ingestion**
* Mastered event-driven orchestration + transformation across GCP services

---

## 📂 Project Structure

```
flight-booking-data-processing/
├── airflow_job.py              # Airflow DAG (Cloud Composer)
├── spark_transformation_job.py # PySpark logic (Dataproc)
├── README.md                   # Documentation
└── sample_data/
    └── flight_bookings.csv     # Example input file
```

---

## 👨‍💻 Author

**Pankaj**
Aspiring GCP Data Engineer | Python • Spark • BigQuery • Cloud-Native Pipelines

---

