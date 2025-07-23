
# Azure Data Factory Project: REST API + Conditional Data Copy Pipeline

## Overview

This project consists of two major pipelines in Azure Data Factory (ADF):
1. **REST API Pipeline** - Fetches country data from the REST Countries API and saves each country's data as a JSON file in Azure Data Lake Storage (ADLS).
2. **Conditional DB Copy Pipeline** - Checks if the customer table has more than 500 records and copies it to ADLS. If the record count is more than 600, it triggers a child pipeline to copy product data.

## Details

### 1. Country Data Fetch Pipeline
- **Data Source:** https://restcountries.com/v3.1/name/{country}
- **Countries:** India, US, UK, China, Russia
- **Output:** JSON files named `india.json`, `us.json`, etc. stored in ADLS
- **Trigger:** Runs twice daily at 12:00 AM and 12:00 PM IST using a time-based trigger.

### 2. Customer and Product Data Copy Pipeline
- **Step 1:** Lookup activity queries record count from `customer` table.
- **Step 2:** If record count > 500, data is copied to ADLS.
- **Step 3:** If record count > 600, a child pipeline is called to copy data from the `product` table.
- **Parameter Passing:** Customer count is passed to the child pipeline via pipeline parameters.

## Files Included
- `CountryDataPipeline.json`: ADF pipeline definition for REST API extraction
- `CustomerProductPipeline.json`: ADF parent pipeline to check and copy customer data
- `ProductChildPipeline.json`: ADF child pipeline to copy product data
- `TriggerCountryPipeline.json`: Time trigger definition for country pipeline

## Deployment Instructions
1. Open Azure Data Factory Studio.
2. Use "Manage hub" to import triggers and "Author hub" to import pipelines.
3. Deploy pipelines using the Import ARM template or manually copy-paste JSON.
4. Ensure Linked Services for REST API, Azure SQL DB, and ADLS are correctly configured.

