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



### Extract
Using Data Factory, we establish a source (the raw files from this repository) and the sink (the staging area where the data will be sent).
![Project architecture](https://github.com/Okunade-Gbenga/e-commerce-azure-databricks/blob/main/extract.png)


### TRANSFORM
After the data has been inserted into the first staging area, we will use Databricks to transform the data through cleaning and EDA. Databricks goes in this process with the use of a notebook that is also linked in this repository.

The majority of the tables were already pretty decent in terms of cleaning, there were just some minor datatype changes for a better schema representation of the columns. There was one exception with the table "order_reviews", there were some problematic rows that were giving error messages when Synapse was trying to read it, those rows had double quotes in them, so I managed to delete all of those rows.

Once the data is transformed, it is then sent to another staging area where it will be ingested by Synapse.

### LOAD
Data is now loaded into Synapse, we will make this data become relational by creating an ERD so we can query some answers from it.
![Project architecture](https://github.com/Okunade-Gbenga/e-commerce-azure-databricks/blob/main/load.png)


We can now query some answers using SQL.

```
checking the states that have the biggest amount of customers
SELECT [customer_state], COUNT(*) AS [total]
 FROM [ecommerceDB].[dbo].[customers]
 GROUP BY [customer_state]
 ORDER BY [total] DESC
```
![result](https://github.com/Okunade-Gbenga/e-commerce-azure-databricks/blob/main/result1.jpg)


```
-- on the "order_payments" table, find the average payment of the payment type and the number of times it was used, from top to bottom
SELECT [payment_type], COUNT(*) AS [total], ROUND(AVG([payment_value]),2) AS [average_payment]
 FROM [ecommerceDB].[dbo].[order_payments]
 WHERE [payment_type] != 'not_defined'
 GROUP BY [payment_type]
 ORDER BY [total] DESC
```

![result2](https://github.com/Okunade-Gbenga/e-commerce-azure-databricks/blob/main/result3.jpg)

```
-- for testing the joins: join the "order_paymets", "orders", "order_items", and "products" tables and get the "order_status", "payment_type", average price in descending order, and the "product_category_name"
SELECT TOP (100) o.[order_status], op.[payment_type], AVG(oi.[price]) AS [average_price], p.[product_category_name]
FROM [ecommerceDB].[dbo].[orders] o
JOIN [ecommerceDB].[dbo].[order_payments] op ON o.[order_id] = op.[order_id]
JOIN [ecommerceDB].[dbo].[order_items] oi ON o.[order_id] = oi.[order_id]
JOIN [ecommerceDB].[dbo].[products] p ON oi.[product_id] = p.[product_id]
GROUP BY o.[order_status], op.[payment_type], p.[product_category_name]
ORDER BY [average_price] DESC;
```
![result2](https://github.com/Okunade-Gbenga/e-commerce-azure-databricks/blob/main/result2.jpg)


## Dashboard

Before we start making our dashboards, we have to establish a connection between Power Bi and Synapse. To do that, download Power Bi desktop and grab the "Serverless SQL endpoint" inside the resource group that you are using.

