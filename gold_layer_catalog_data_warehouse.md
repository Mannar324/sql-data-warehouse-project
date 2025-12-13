# Gold Layer Catalog – Data Warehouse

## Overview
The **Gold Layer** is the business-ready layer of the data warehouse. It is optimized for **analytics, reporting, and BI consumption**, and follows a **star schema** design with fact and dimension tables.

This layer contains curated, validated, and enriched data intended for direct use by analysts and reporting tools.

---

## Gold Layer Tables

### gold.dim_customers

**Purpose**  
Stores customer demographic and geographic information used for customer-level analysis.

**Grain**  
One row per customer.

**Schema**

| Column Name | Data Type | Description |
|------------|----------|-------------|
| customer_key | INT | Surrogate key uniquely identifying each customer record. |
| customer_id | INT | Business identifier for the customer. |
| customer_number | NVARCHAR(50) | Alphanumeric customer reference number. |
| first_name | NVARCHAR(50) | Customer first name. |
| last_name | NVARCHAR(50) | Customer last name. |
| country | NVARCHAR(50) | Customer country of residence. |
| marital_status | NVARCHAR(50) | Marital status of the customer. |
| gender | NVARCHAR(50) | Gender of the customer. |
| birthdate | DATE | Customer date of birth. |
| create_date | DATE | Date the customer record was created. |

---

### gold.dim_products

**Purpose**  
Contains descriptive product information and classification attributes for product-level analysis.

**Grain**  
One row per product.

**Schema**

| Column Name | Data Type | Description |
|------------|----------|-------------|
| product_key | INT | Surrogate key uniquely identifying each product. |
| product_id | INT | Business identifier for the product. |
| product_number | NVARCHAR(50) | Alphanumeric product code. |
| product_name | NVARCHAR(50) | Descriptive name of the product. |
| category_id | NVARCHAR(50) | Identifier for the product category. |
| category | NVARCHAR(50) | High-level product category. |
| subcategory | NVARCHAR(50) | Detailed product subcategory. |
| maintenance_required | NVARCHAR(50) | Indicates whether maintenance is required. |
| cost | INT | Base cost of the product. |
| product_line | NVARCHAR(50) | Product line or series. |
| start_date | DATE | Date the product became available. |

---

### gold.fact_sales

**Purpose**  
Stores transactional sales data used as the main fact table for sales analysis.

**Grain**  
One row per product per sales order.

**Schema**

| Column Name | Data Type | Description |
|------------|----------|-------------|
| order_number | NVARCHAR(50) | Unique identifier for the sales order. |
| product_key | INT | Foreign key referencing gold.dim_products. |
| customer_key | INT | Foreign key referencing gold.dim_customers. |
| order_date | DATE | Date the order was placed. |
| shipping_date | DATE | Date the order was shipped. |
| due_date | DATE | Payment due date. |
| sales_amount | INT | Total sales value for the line item. |
| quantity | INT | Quantity of units sold. |
| price | INT | Unit price of the product. |

---

## Data Model Relationships

- gold.fact_sales.customer_key → gold.dim_customers.customer_key
- gold.fact_sales.product_key → gold.dim_products.product_key

These relationships enable analytical queries across customers, products, and sales metrics.

---

## Usage

The Gold Layer is intended for:

- Business intelligence dashboards
- Ad-hoc analytical queries
- Sales and customer performance reporting
- Time-series and trend analysis

---

## Notes

- Tables are optimized for read-heavy workloads.
- Surrogate keys are used to ensure stable joins.
- This layer should not be used for transactional updates.

