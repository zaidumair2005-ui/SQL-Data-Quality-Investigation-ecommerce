
# E-Commerce Data Quality Analysis Project

## Overview
This project focuses on assessing and improving the data quality of a
e-commerce dataset. The analysis involves identifying potential
data quality issues such as missing values, duplicates, and inconsistencies
using SQL, with further processing planned using Python and Excel.

## Dataset
- Brazilian E-Commerce Public Dataset from kaggle

## Tools & Technologies
- SQL (MySQL)
- DBeaver (SQL Client)

## Initial Data Exploration

The first phase of the project involved setting up the database and
performing initial exploratory queries to understand the structure
and size of the data.

### Database Setup
- Imported customer and order datasets into a MySQL database
- Verified successful data import and table accessibility

### Exploratory Analysis
The following exploratory checks were performed:
- Counted total number of customers 
- Counted total number of orders
- Executed basic `SELECT` queries to inspect table structure and sample records

**Key Findings:**
- Total customers: 96,069
- Total customers records: 99,441
- Total orders: 99,441
### Customer Uniqueness Validation

However, only 96,069 distinct `customer_unique_id`values were identified.
This indicates that some real customers appear multiple times in the dataset,
which is expected behavior, as a single customer may place multiple orders
and therefore be associated with multiple system-generated customer IDs.

These checks confirmed that the data was correctly loaded and ready
for further data quality validation.

## Next Steps
- Identify NULL values in key columns
- Detect duplicate records
- Validate primary and foreign key relationships
- Perform consistency checks across related tables
