# Nike Sales Cloud Data Analytics Pipeline

This project implements an end-to-end cloud-based data analytics pipeline for a Nike USA retail sales dataset using Apache Spark. It covers data ingestion, cleaning, transformation, enrichment with dimension tables, and advanced analytical queries such as regional performance, product analysis, clustering, and rolling-demand features.

---

## Project Overview

The goal of this project is to turn a raw transactional sales extract into a clean, analysis-ready dataset and then derive business insights at scale. The pipeline is built around Spark DataFrames and Spark MLlib so it can later be deployed on managed platforms such as Databricks, AWS EMR, or Google Cloud Dataproc with minimal changes.

Key capabilities:

- Load raw Nike sales and mapping tables (departments, categories, regions, product types, brands) from Excel/CSV.  
- Clean the data (e.g., handle missing product size values) and validate there are no remaining nulls in critical fields.  
- Enrich the fact table by joining all dimension tables to replace numeric IDs with business-readable names.  
- Engineer features such as `Year`, `Month`, and `Price_Band` to support time-series and pricing analysis.  
- Run advanced analytics:
  - Sales and revenue by region, category, brand, and department.
  - Monthly sales trends and 3?month rolling averages by region.
  - Best-selling product types and top brands by revenue.
  - KMeans clustering for buying pattern segmentation.
  - Margin optimisation by product type and price bands.
  - Correlation analysis between price, units sold, and operating margin.

---

## Technologies Used

- **Apache Spark (PySpark)** for distributed data processing and SQL-style transformations.  
- **Spark MLlib** for clustering (KMeans) and basic feature preparation.  
- **Python / Jupyter / Google Colab** as the development environment and notebook interface.  
- **Pandas** (only in early stages) for initial Excel reading if needed.

---

## Data Pipeline

The pipeline implemented in `Cloud_Project.ipynb` follows these main steps.

1. **Ingestion**  
   - Load the main sales table (`NikeVentas`) and mapping tables (`Departamentos`, `Categorias`, `Region`, `TipoProducto`, `Marca`) into Spark DataFrames.

2. **Cleaning**  
   - Identify rows with missing `PRODUCTSIZE` values and drop them (very small fraction of data).  
   - Re-check for nulls across all columns to confirm data quality.

3. **Enrichment via Joins**  
   - Join the fact table with:
     - Department dimension ? `Department` (Men, Women, Kids).  
     - Category dimension ? `Category` (Clothing, Shoes, Accessories and Equipment).  
     - Region dimension ? `RegionName` (Northeast, South, West, Midwest, Southeast).  
     - Product Type dimension ? `ProductType` (APPAREL, FOOTWEAR, EQUIPMENT, ACCESSORIES).  
     - Brand dimension ? `Brand` (Nike, Nike Sportswear, Jordan, etc.).

4. **Feature Engineering**  
   - Extract `Year` and `Month` from `INVOICE DATE` for time-series analysis.  
   - Define `Price_Band` buckets (e.g., `<50`, `50-99`, `100-149`, `150+`) for price elasticity analysis.

5. **Analytics & Modelling**  
   - **Aggregations:** Region-level units and revenue, category/brand/department performance, price-band performance.  
   - **Clustering:** KMeans with features such as `Units Sold`, `Price per Unit`, and `Operating Margin` to segment transaction patterns.  
   - **Rolling averages:** 3?month rolling average of units sold per region as a simple forecasting feature.  
   - **Correlation:** Compute correlation matrix between price, units sold, and operating margin.

---

## How to Run

1. **Environment Setup**  
   - Open `Cloud_Project.ipynb` in Google Colab or Jupyter.  
   - Ensure `pyspark` is installed (the notebook includes an installation cell for Colab).

2. **Data Placement**  
   - Upload `nikedata.xlsx` (or CSVs) to the Colab session or mount from cloud storage.  
   - If using CSVs, update the file paths in the ingestion cells accordingly.

3. **Execution Order**  
   - Run the notebook from top to bottom:
     1. Install/import libraries.  
     2. Load raw data.  
     3. Run cleaning and quality checks.  
     4. Execute dimension joins and feature engineering.  
     5. Run analytical cells (regional sales, clustering, rolling averages, etc.).

4. **Outputs**  
   - The notebook prints aggregated tables and statistics directly.  
   - You can export the final enriched Spark DataFrame to CSV/Parquet if you wish to use it in BI tools.

---

## Use Cases and Insights

The code and pipeline are designed to support typical retail analytics questions, such as:

- Which regions and departments drive most of the sales volume and revenue?  
- How do sales evolve month by month, and what is the smoothed rolling trend?  
- Which product types and brands are top performers by volume and by revenue?  
- Are there distinct customer or transaction segments based on price, volume, and margin?  
- How does performance vary by price band, and where are the high-margin opportunities?  

These insights can inform decisions around marketing focus, inventory planning, and pricing strategies.

---

## License and Academic Use

This project was developed as part of a Master’s level Cloud Technologies / Data Analytics module and is intended for educational and demonstrative purposes.
