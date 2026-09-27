# Azure Data Factory: Credit Risk Medallion Pipeline

## Project Overview
This mini project demonstrates an end-to-end Data Engineering ETL (Extract, Transform, Load) pipeline built in Azure. It processes loan applicant data to calculate a critical financial risk metric (Debt-to-Income ratio) and aggregates the data for executive reporting. 

The pipeline utilizes a **Medallion Architecture** (Bronze, Silver, Gold) hosted on Azure Data Lake Storage Gen2, with all data movement and transformations orchestrated via Azure Data Factory (ADF) Mapping Data Flows.

## Architecture & Tooling
* **Orchestration & Transformation:** Azure Data Factory (Pipelines & Mapping Data Flows)
* **Storage:** Azure Data Lake Storage Gen2 (Hierarchical Namespaces configured)
* **Data Format:** DelimitedText (CSV)
* **Logic/Math:** ADF Expression Builder (Derived Column, Aggregates)

*(Tip: Add a screenshot here showing your Bronze, Silver, and Gold containers, or a diagram of your pipeline)*

## Data Pipeline Flow

### 1. Ingestion (Bronze Layer)
* **Source:** Raw mock loan data (`customer_id`, `monthly_income_zar`, `monthly_debt_zar`) ingested as a CSV file.
* **Storage:** Landed in the `bronze` container of the ADLS Gen2 storage account without modification to preserve the raw state.

### 2. Transformation (Silver Layer)
* **Process:** An ADF Data Flow reads the Bronze data. A **Derived Column** transformation is applied to calculate the Debt-to-Income (DTI) percentage. 
* **Business Logic:** `(toInteger(monthly_debt_zar) / toInteger(monthly_income_zar)) * 100`
* **Storage:** The cleaned, row-level data with the newly appended `DTI_Percentage` column is written to the `silver` container.

### 3. Aggregation (Gold Layer)
* **Process:** The pipeline branches from the transformation step into an **Aggregate** transformation. 
* **Business Logic:** The pipeline calculates the overall portfolio risk by deriving `avg(DTI_Percentage)`.
* **Storage:** The summarized metric is written to the `gold` container, ready to be consumed by BI tools or executive dashboards.

## Project Screenshots
ADF Data Flow canvas showing the branching paths to Silver and Gold:
<img width="1562" height="608" alt="image" src="https://github.com/user-attachments/assets/87e91cf6-91d7-4450-a605-08fd2939aa9e" />

Data Preview tab showing the calculated DTI column:
<img width="1614" height="699" alt="image" src="https://github.com/user-attachments/assets/b5060cee-9119-4b55-b8a0-cbf49a9acba7" />

Successful Pipeline Debug run output:
<img width="1634" height="765" alt="image" src="https://github.com/user-attachments/assets/b9271821-cf8b-4fa9-90bb-6967e1332dec" />

