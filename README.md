
# E-Commerce Data Quality Analysis Project

## Overview
This project focuses on assessing and improving the data quality of a
e-commerce dataset. The analysis involves identifying potential
data quality issues such as missing values, duplicates, and inconsistencies
using SQL, with further processing planned using Python and Excel.

## Dataset
- Brazilian E-Commerce Public Dataset from kaggle
- There are 8 different CSV files in one folder that are liked to one or another.
- Following are the names of those files
  olist_customers_dataset
  olist_geolocation_dataset
  olist_order_items_dataset
  olist_order_payments_dataset
  olist_order_reviews_dataset
  olist_orders_dataset
  olist_products_dataset
  olist_sellers_dataset

## Tools & Technologies
- SQL (MySQL)
- DBeaver (SQL Client)

### Database Setup
- Imported customer and order datasets into a MySQL database
- Verified successful data import and table accessibility

## Initial Data Exploration
The first phase of the project involved setting up the database and
performing initial exploratory queries to understand the structure
and size of the data.

### Exploratory Analysis
The following exploratory checks were performed:
- Counted total number of customers 
- Counted total number of orders
- Executed basic `SELECT` queries to inspect table structure and sample records

###**Table Structure**
Table         Total_Rows
customers	    99441
geolocation	  1000163
Order Items 	112650
Order Payment	103886
Order Reviews	77916
Orders	      99441
Products    	32951
Sellers     	3095

**Key Findings:**

- Customers: 96,069
- Total customers records: 99,441
- Total orders: 99,441
- Even though there are one million record of geolocation table, that is an expected number because same zip code can appear many times with distinct latitude and longitude values
- The product ID itself is unique per product. The product is repeated in transactional tables.This is expected behavior, not a data quality issue (Product_id > Distinct_product_id)
- Fortunately 77 % is the satisfaction rate among customers
  
### Customer Uniqueness Validation

However, only 96,069 distinct `customer_unique_id`values were identified.
This indicates that some real customers appear multiple times in the dataset,
which is expected behavior, as a single customer may place multiple orders
and therefore be associated with multiple system-generated customer IDs.

These checks confirmed that the data was correctly loaded and ready
for further data quality validation.

### Checking for nulls in every table
- Customer_id         null_values = 0
- Orders_id           null_values = 0
- customer_unique_id   null_values = 0
- Order(table)         null_values = 0
- Photo_quality        null_values = 610
- Product_weight       null_values = 2
- seller(table)        null_values = 0

### Duplicate Analysis

Compared total rows vs. distinct business identifiers

Validated that:
- Customers, Orders, Products, and Sellers contain no invalid duplicates
- Repeated order items reflect legitimate multi-item purchases
- Multiple customer records are business-valid due to address or delivery changes

### Relational Integrity Checks

Confirmed:

- One-to-many relationships between customers and orders

- One-to-many relationships between orders and order items

- Proper linkage across product and seller tables

### Review Quality Metrics

- Calculated customer satisfaction percentages using review scores

- Verified rating distributions for analytical consistency

### Key Findings

- No critical data quality issues detected

- All observed repetitions are business-justified

- Dataset is structurally sound and analytics-ready




