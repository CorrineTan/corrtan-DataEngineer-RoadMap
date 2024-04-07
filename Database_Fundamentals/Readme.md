# DB Design
## OLTP vs OLAP
- OLAP: complex data analysis and reporting (e.g. Redshift)
- OLTP: transcational process, real-time updates 
(e.g. both RDS and Aurora are for OLAP and OLTP)

Example of Usage: <br>
A retail company operates hundreds of stores across the country, their db track sales, inventory, customer data, other key metrics. 
<br>OLTP is used to store transcation in real time, update inventory levels, manage customer accounts. Each store connected to central db to update inventory levels in real time as products are sold, and manage customer accounts (track loyal points, payment info, process returns). 
<br>OLAP is used to analyze data collected by OLTP like generating reports on sales trends, inventory levels, customer demographics, other key metrics. The report can be used to perform complex queries on large volume of historical data to identify patterns and trends that can inform business decisions. 

DB Design:
- OLAP: Dimensional Modelling(Star Schema or Snowflake Schema) - fact tables(defined by use-case based on busines requirements, metrics, update regularly, connected to dimension tables via foreign keys) and dimensional tables(attributes, doesn't change often). They are denormalized - allows redundancy, but significantly improve query performance by reducing number of joins.
- OLTP: emphasize normalization (reduce redundancy and dependency by dividing large tables into small tables) and ACID(atomic, consistent, isolated, durable), extensive used of indexes to speed up query.

## Star Schema vs Snowflake Schema
- Star Schema: fact table - large central table, stores quantitiative info (metrics/measures), surrounded by smaller dimension tables stored descriptive attributes related to measurements.
<br> Example:
<br> Fact table:
    - SaleID (Primary Key): A unique identifier for each sales transaction.
    - ProductID: References the Product dimension table.
    - CustomerID: References the Customer dimension table.
    - StoreID: References the Store dimension table.
    - DateID: References the Date dimension table.
    - QuantitySold: The number of items sold in the transaction.
    - TotalSaleAmount: The total amount of money exchanged in the transaction.
- Snowflake Schema: more complex than star schema - as dimension tables are normalized (break down into additional tables)
<br> Dimension tables:
    - Product Dimension
Columns:
ProductID (Primary Key): A unique identifier for each product.
ProductName: The name of the product.
Category: The category to which the product belongs.
Price: The retail price of the product.
    - Customer Dimension
Columns:
CustomerID (Primary Key): A unique identifier for each customer.
CustomerName: The name of the customer.
Location: The geographical location of the customer.
Segment: The market segment to which the customer belongs.
    - Store Dimension
Columns:
StoreID (Primary Key): A unique identifier for each store.
StoreName: The name of the store.
StoreLocation: The geographical location of the store.
Manager: The name of the store manager.
    - Date Dimension
Columns:
DateID (Primary Key): A unique identifier for each date.
Date: The actual date of the transaction.
Weekday: The day of the week.
Month: The month of the year.
Quarter: The quarter of the year.
Year: The year.

## Data Lake vs Data Warehouse
- Data Lake: 
  - Support different data structure(structured data, unstructured data, semi-structured data)
  - Schema-on-read: schema is created as data is being read (so data catalog is crucial otherwise just data swamp)
- Data Warehouse: 
  - Storing unstructured data in traditional db is expensive (data lake storage is cheaper because it's using object storage as opposed to block/file storage)
  - Schema-on-write: pre-defined schema

# SQL Specific
## Stored Procedure