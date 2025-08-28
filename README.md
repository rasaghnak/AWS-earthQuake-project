# AWS Datapipeline in Medallion Architecture

# AWS Medallion Data Pipeline Demo

## Overview

This project shows a **basic data pipeline** built on AWS using the **Medallion Architecture (Bronze → Silver → Gold)** pattern.
We use a free public API (`https://jsonplaceholder.typicode.com/posts`) as the data source, then process the data through three stages:

* **Bronze (Raw):** Raw API data is stored in S3 exactly as received.
* **Silver (Clean):** Data is cleaned and standardized into a consistent CSV format.
* **Gold (Curated):** Data is aggregated into simple metrics ready for reporting.

---

## Why Medallion Architecture?

The **Medallion Architecture** is a best practice for organizing data pipelines because:

1. **Clarity & Structure:** Splits data into clear layers — Raw, Cleaned, and Business-Ready.
2. **Traceability:** Raw data (Bronze) is always available if something goes wrong downstream.
3. **Reusability:** Silver data can be reused for multiple analytics or machine learning use cases.
4. **Simplicity for Teams:** Easy for business, data engineers, and analysts to understand where to look.

In short: it helps turn messy raw data into trusted, usable business insights step by step.

---

## General Use Case

This pattern is widely used across industries to:

* Ingest data from APIs, files, or databases.
* Clean and standardize it into consistent formats.
* Build curated business reports or feed dashboards & machine learning.

Typical examples:

* **E-commerce:** API → Raw sales orders → Clean data → Gold metrics for daily revenue dashboards.
* **HR Analytics:** Employee survey API → Bronze responses → Silver cleaned data → Gold engagement scores.
* **Finance:** Stock price feeds → Bronze trades → Silver validated feeds → Gold risk metrics.

Our project is a simplified demo, but the same flow is used in production systems with millions of records.

---

## AWS Services Used

* **Amazon S3:** Central storage for Bronze, Silver, Gold data.
* **AWS Lambda:** Serverless functions to move and transform data.
* **AWS Step Functions:** Orchestration — runs the pipeline in order.
* **Amazon EventBridge:** Scheduling — triggers pipeline daily.
* *(Optional)* **Amazon Athena:** Query data with SQL.

---

## Data Flow

1. **Extract (Bronze):** A Lambda fetches raw posts from the API → stores JSON in S3 (Bronze).
2. **Transform (Silver):** Another Lambda cleans JSON → saves CSV in S3 (Silver).
3. **Aggregate (Gold):** Final Lambda groups posts by user → saves metrics in S3 (Gold).
4. **Query:** Optional Athena queries or dashboards (e.g., QuickSight).

---

## How to Run

1. Create one S3 bucket, e.g. `mdn-demo-datalake-<yourname>-us-east-1`.
2. Deploy the 3 Lambdas from `src/`.

   * Runtime: Python 3.12
   * Handler: `lambda_function.lambda_handler`
   * Attach IAM role with provided policies.
   * Add environment variables (see code comments).
3. Create the Step Functions state machine using `infra/step-functions/state-machine.json`.
4. (Optional) Add a daily EventBridge schedule.
5. (Optional) Run Athena SQL from `athena/` to query the Silver and Gold tables.

---

## Benefits

* **Simple & Clear:** Great for demos and training.
* **Scalable:** Can grow with more data sources or transformations.
* **Industry-Relevant:** Shows a real-world architecture that HR, managers, and engineers can all relate to.

