# ✈️ Flight Operations Data Pipeline

### 🏅 Medallion Architecture (Bronze → Silver → Gold)

---

## 📌 Project Overview

This project implements an **end-to-end data pipeline** using **Apache Airflow** and **Snowflake**, designed according to the **Medallion Architecture** (Bronze, Silver, and Gold layers).

The pipeline ingests raw flight operations data, processes it through multiple transformation stages, and loads aggregated **business KPIs** into a cloud data warehouse for analytics and reporting.

Developed as a **Data Engineering portfolio project**, this repository focuses not only on final outputs, but also on **real-world challenges**, **debugging scenarios**, and **technical decision-making** commonly encountered in production-grade data pipelines.

---

## 🏗️ Architecture Overview

### 🔄 Data Flow

#### 🥉 Bronze Layer

- Ingests raw flight data in JSON format
- Stores data in its original form to ensure traceability and enable reprocessing

#### 🥈 Silver Layer

- Performs data cleansing and normalization
- Applies schema standardization and data quality validation

#### 🥇 Gold Layer

- Aggregates curated data into business-level KPIs
- Produces analytics-ready datasets

#### 🏢 Data Warehouse

- Loads Gold-layer data into **Snowflake**
- Uses `MERGE` (upsert) logic to ensure idempotent and reliable data loads

---

## 📈 Dashboard & Analytics

The final output of this pipeline feeds into a visualization layer (e.g., Streamlit, Tableau, or Snowflake Dashboards) to monitor real-time flight metrics processed by the **Gold Layer**:

![Flight Activity Dashboard](Flight_Activity_dashboard.png)

### 📊 Key Metrics Monitored:

- **Global Volume:** Real-time count of active flights and aircraft on the ground.
- **Traffic by Country:** High-volume traffic analysis by origin country (e.g., US, Canada, Ireland).
- **Operational Efficiency:** Average fleet airspeed velocity trends over time.

### ⚙️ Orchestration

- **Apache Airflow**, running in a Docker-based environment, is used to orchestrate and monitor the pipeline execution.

---

## 🧰 Technology Stack

- 🛠 **Apache Airflow** – Workflow orchestration
- 🐍 **Python** – Data processing and transformation
- 🧪 **Pandas** – Data manipulation
- ❄️ **Snowflake** – Cloud data warehouse
- 🐘 **PostgreSQL** – Airflow metadata database
- 🐳 **Docker & Docker Compose** – Containerized execution environment

---

## 📂 Project Structure

```text
.
├── dags/
│   └── flights_ops_medallion_pipe.py
├── scripts/
│   ├── bronze_ingest.py
│   ├── silver_transform.py
│   ├── gold_aggregate.py
│   └── load_gold_to_snowflake.py
├── data/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── docker-compose.yml
└── requirements.txt
```

---

## 🔗 DAG Design

The Airflow DAG is designed with **linear task dependencies** to maintain data integrity across pipeline stages.

- ⏱ **Schedule:** Every 30 minutes
- 🔁 **Retry Policy:** Configurable per task
- 🔄 **XCom:** Used to pass file paths between tasks

---

## 🧊 Snowflake Data Model

### 📊 Table: `FLIGHT_KPIS`

| Column         | Description                      |
| -------------- | -------------------------------- |
| window_start   | Aggregation window timestamp     |
| origin_country | Flight origin country            |
| total_flights  | Total number of flights          |
| avg_velocity   | Average flight velocity          |
| on_ground      | Number of aircraft on the ground |
| load_time      | Data load timestamp              |

🔑 **Primary Key:** `(window_start, origin_country)`

---
