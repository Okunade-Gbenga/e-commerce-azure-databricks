# E-commerce-azure-databricks
## Introduction
In this project, we will be using some ETL, analytics, and BI tools on Microsoft Azure cloud in the company of Databricks and Power Bi. We will use the E-Commerce Public Dataset by Olist to create a relational database for analysis.
#### ARCHITECTURE
![Project architecture](https://github.com/Okunade-Gbenga/e-commerce-azure-databricks/blob/main/data%20acrhiteture.png)

### Technologies used-
1. **Programming language- Python**
2. **Scripting language - SQL**
3. **Azure data factory** - Used for the ingestion of the raw files that you can find in this repository. It will then send those raw files to a staging area in a data lake inside a Storage Account
4. **Azure datbricks** - Used for data cleaning and some EDA (Exploratory Data Analysis). This section is important because it will make the correct schema for further data analysis on Synapse.
5. **Azure Data lake gen2** - These will be the staging areas for the data. The first staging area will be the raw data that will be ingested by Databricks. The second staging area will be the transformed data by Databricks that will be ingested by Azure Synapse.
6. **Azure Synapse** - This is the analytics tool offered by Azure. We can use the transformed data to make a relational database model and query data for analytics

7. **PowerBi**- Used for presenting dashboards with the data that is the relational format
