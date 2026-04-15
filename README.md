===========================================================
PROJECT: STEAM ANALYTICS - END-TO-END BIG DATA PIPELINE
===========================================================
Author: Luis Miguel Portugal Kegel
Course: Big Data
===========================================================

PROJECT DESCRIPTION:
This project implements a complete Big Data pipeline following the Medallion 
Architecture (Bronze, Silver, and Gold layers). It processes over 41 million 
Steam review records to generate business insights and train high-scale 
predictive Machine Learning models.

TECH STACK:
- Docker & Docker Compose (Containerization & Microservices)
- Apache Spark / PySpark (Distributed Processing & MLlib)
- Spark Structured Streaming (Real-time Processing)
- PostgreSQL (Gold Layer / Curated Data Storage)
- Metabase (Enterprise Business Intelligence & Dashboards)
- Jupyter Lab (Development Environment)

---
1. PREREQUISITES
---
- Docker and Docker Compose installed.
- Minimum 8GB of RAM allocated to Docker (recommended 12GB+ for 41M rows).
- Steam Dataset from Kaggle ("Game Recommendations on Steam").
- Place raw files in the following directory structure:

  /steam_bigdata_project
  │-- docker-compose.yml
  │-- data/
  │   └── raw/
  │       │-- games.csv
  │       │-- games_metadata.json
  │       │-- recommendations.csv
  │       │-- users.csv

---
2. ENVIRONMENT SETUP (DOCKER)
---
Open your terminal in the project root folder and run:

    docker-compose up -d

This will deploy three integrated services:
1. 'spark-jupyter' (Port 8888): PySpark environment.
2. 'postgres-db' (Port 5432): Relational storage for the Gold Layer.
3. 'metabase' (Port 3000): BI Tool for data visualization.

---
3. EXECUTION WORKFLOW (JUPYTER NOTEBOOKS)
---
Access Jupyter Lab at http://localhost:8888 and execute the notebooks in order:

Step 1: [01_ingestion.ipynb]
   - Scans the raw data and creates an ingestion audit log.
   
Step 2: [02_storage_setup.ipynb]
   - Prepares the PostgreSQL schema and the 'game_insights' table.

Step 3: [03_spark_batch.ipynb]
   - The Spark engine processes 41 million records.
   - Performs massive joins, cleans data, and calculates business metrics.
   - Saves to Parquet (Silver Layer) and PostgreSQL (Gold Layer).

Step 4: [04_live_stream_simulator.ipynb]
   - Starts a background process to simulate a real-time data feed.

Step 5: [05_spark_streaming.ipynb]
   - Launches a Spark Structured Streaming query to process live data.

Step 6: [07_machine_learning.ipynb]
   - Trains a Logistic Regression model on 32 million rows.
   - Predicts recommendation probability based on playtime and price.

---
4. ANALYTICS & DASHBOARD (METABASE)
---
1. Go to http://localhost:3000 in your browser.
2. Setup the database connection using these credentials:
   - Host: postgres-db (Crucial: use the container name, not localhost)
   - Port: 5432
   - Database: steam_analytics
   - User: admin / Password: admin
3. Create Visualizations:
   - Bar Chart: Top 10 Games by Review Volume.
   - Scatter Plot: Average Playtime vs. Recommendation Rate.

---
5. IMPORTANT NOTES
---
- The 'recommendations.csv' file is ~1.9GB. Ensure your machine has enough 
  disk space before running the batch process.
- Internal networking: Metabase connects to Postgres via the 'postgres-db' 
  hostname because they share the same Docker network.
- Spark UI: While a job is running, you can monitor the cluster 
  performance at http://localhost:4040.
===========================================================
