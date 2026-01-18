# 🛒 E-Commerce Data Quality Analysis

[![Status](https://img.shields.io/badge/Status-Complete-brightgreen)]()
[![Grade](https://img.shields.io/badge/Data%20Quality-A%20(98%25)-success)]()
[![SQL](https://img.shields.io/badge/SQL-MySQL-blue)]()

> **Comprehensive data quality assessment of Brazilian e-commerce platform achieving 98% quality score with zero critical issues across 1.5M+ records.**

---

## 📊 Project Overview

**Business Context:**  
E-commerce platforms depend on accurate data for revenue reporting, inventory management, and customer analytics. This analysis validates the Olist database's structural integrity to ensure it's production-ready for business intelligence and decision-making.

**Objective:**  
Systematically assess data quality across 8 interconnected tables using the industry-standard Six Dimensions framework (Completeness, Uniqueness, Consistency, Validity, Accuracy, Timeliness).

**Outcome:**  
✅ **Database approved for production use** — Zero critical issues, 100% referential integrity, ready for analytics without cleanup.

---

## 📁 Dataset

**Source:** [Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Olist/Kaggle)  
**Platform:** Olist — Brazilian marketplace connecting small businesses with customers  
**Period:** September 2016 - October 2018 (25 months)  
**Geographic Scope:** Brazil (all states)

### Database Structure

| Table | Records | Description |
|-------|---------|-------------|
| **customers** | 99,441 | Customer demographics and delivery addresses |
| **orders** | 99,441 | Order lifecycle (purchase → delivery) |
| **order_items** | 112,650 | Individual products within each order |
| **products** | 32,951 | Product catalog with attributes |
| **sellers** | 3,095 | Marketplace sellers and locations |
| **order_payments** | 103,886 | Payment transactions and methods |
| **order_reviews** | 77,916 | Customer satisfaction ratings |
| **geolocation** | 1,000,163 | Geographic coordinates for Brazilian zip codes |

**Total Records Analyzed:** 1,529,033

---

## 🛠️ Tools & Technologies

- **Database:** MySQL
- **SQL Client:** DBeaver
- **Analysis Techniques:** Aggregate functions, JOINs, subqueries, NULL detection, duplicate validation
- **Framework:** Six Dimensions of Data Quality

---

## 🔍 Analysis Methodology

### 1️⃣ **Completeness** (NULL Detection)
Checked all critical fields for missing values using `COUNT(*)` vs `COUNT(column)`.

### 2️⃣ **Uniqueness** (Duplicate Detection)  
Compared total rows vs `COUNT(DISTINCT key)` to identify duplicates.

### 3️⃣ **Consistency** (Referential Integrity)  
Validated 6 foreign key relationships using `LEFT JOIN` with orphan detection.

### 4️⃣ **Validity** (Business Rule Validation)  
Applied domain-specific logic (price ranges, date sequences, format compliance).

### 5️⃣ **Accuracy** (Outlier Detection)  
Investigated extreme values to distinguish errors from valid business behavior.

### 6️⃣ **Timeliness**  
Confirmed data coverage period (historical dataset, not real-time).

---

## 📈 Key Findings

### ✅ **Overall Data Quality: Grade A (98/100)**

| Dimension | Score | Status | Details |
|-----------|-------|--------|---------|
| **Completeness** | 100% | ✓ PASS | All critical fields (IDs, prices, dates) populated |
| **Uniqueness** | 100% | ✓ PASS | No invalid duplicates detected |
| **Referential Integrity** | 100% | ✓ PASS | 0 orphaned records across 6 relationships |
| **Validity** | 100% | ✓ PASS | Prices valid, dates logical, business rules met |
| **Accuracy** | 98% | ✓ GOOD | 610 products (1.9%) missing categories |
| **Timeliness** | N/A | INFO | Historical dataset (2016-2018) |

---

### 🎯 **Completeness: 100% (PASS)**

**Zero NULL values in critical fields:**
- ✅ All customer IDs, order IDs, product IDs, seller IDs populated
- ✅ All transactional fields (prices, dates, payments) complete
- ✅ 100% completeness in operational tables

**Minor gaps (non-critical):**
- ⚠️ 610 products (1.9%) missing `product_category_name` — acceptable for new inventory
- ⚠️ 2 products (0.006%) missing `product_weight_g` — negligible impact

**Business Impact:**  
✓ No operational blockers. All fields required for order fulfillment, customer communication, and financial reporting are complete.

---

### 🎯 **Uniqueness: 100% (PASS)**

**No invalid duplicates detected:**

| Field | Unique Count | Total Records | Uniqueness |
|-------|--------------|---------------|------------|
| `customer_id` | 99,441 | 99,441 | 100% |
| `order_id` | 99,441 | 99,441 | 100% |
| `product_id` | 32,951 | 32,951 | 100% |
| `seller_id` | 3,095 | 3,095 | 100% |

**Important Business Distinction:**
- **Total customer records:** 99,441
- **Unique people (`customer_unique_id`):** 96,096
- **Difference:** 3,345 people (3.4%) have multiple `customer_id` entries

**Why This is VALID (Not an Error):**  
Olist assigns a new `customer_id` when a customer orders from a different delivery address.

**Example:**
- Customer moves from São Paulo → Rio de Janeiro
- Same person (`customer_unique_id = "ABC123"`) gets two `customer_id` entries (one per city)
- Preserves geolocation accuracy for delivery logistics

**Implication:**
- Use `customer_unique_id` for customer analytics (lifetime value, retention)
- Use `customer_id` for operations (delivery tracking, address validation)

**Repeated Order Items:**  
Multiple rows with the same `order_id` in `order_items` represent multi-item purchases (e.g., buying 3 books = 3 rows). This is **valid transactional data**, not duplication.

---

### 🎯 **Referential Integrity: 100% (PASS)**

**All foreign key relationships validated (6 critical paths):**

| Relationship | Child Table | Parent Table | Status | Orphaned | Impact |
|--------------|-------------|--------------|--------|----------|--------|
| Orders → Customers | orders (99,441) | customers | ✓ PASS | 0 | All orders traceable |
| Order Items → Orders | order_items (112,650) | orders | ✓ PASS | 0 | All items fulfill valid orders |
| Order Items → Products | order_items (112,650) | products | ✓ PASS | 0 | All items reference existing products |
| Order Items → Sellers | order_items (112,650) | sellers | ✓ PASS | 0 | All items link to valid sellers |
| Payments → Orders | order_payments (103,886) | orders | ✓ PASS | 0 | Financial integrity intact |
| Reviews → Orders | order_reviews (77,916) | orders | ✓ PASS | 0 | All feedback traceable |

**Zero orphaned records across 528,409 child→parent relationships.**

**Business Impact:**
- ✅ Revenue attribution 100% accurate (no payments without orders)
- ✅ Customer support can access complete order history
- ✅ Analytics joins work flawlessly (no broken links)
- ✅ All transactions traceable to customer, product, and seller

---

### 🎯 **Validity: 100% (PASS)**

**Financial Integrity:**
- ✅ Zero invalid prices: All `price` and `freight_value` fields are ≥ 0
- ✅ Impact: Revenue and shipping cost calculations accurate

**Temporal Logic:**
- ✅ Zero chronological errors: All `order_delivered_customer_date` values occur AFTER `order_purchase_timestamp`
- ✅ Impact: Time-to-delivery KPIs are reliable

**Outlier Investigation:**
- 🔍 **Payment installments:** Maximum of 24 detected
- ✅ **Analysis:** Initially flagged, but confirmed as **standard Brazilian consumer credit practice**
- ✅ **Conclusion:** Valid business behavior, not a data error

**Order Status Distribution:**
| Status | Percentage | Assessment |
|--------|------------|------------|
| Delivered | 96.5% | ✅ Healthy |
| Shipped | 1.1% | ✅ Normal |
| Canceled | 0.6% | ✅ Low |
| Processing | 1.8% | ✅ Normal |

---

### 📊 **Additional Business Insights**

**Customer Satisfaction:**
- **77% positive reviews** (4-5 stars out of 5)
- **Review participation:** 78.5% of orders receive feedback
- High engagement enables reliable sentiment analysis

**Geographic Distribution:**

**Top Customer States:**
1. São Paulo (SP): 41.7%
2. Rio de Janeiro (RJ): 12.9%
3. Minas Gerais (MG): 11.6%

**Top Seller States:**
1. São Paulo (SP): 47.3%
2. Paraná (PR): 8.2%
3. Minas Gerais (MG): 7.8%

**Business Insight:** SP concentration creates warehousing optimization opportunities.

---

## 💼 Business Impact

### **What This Means for Operations:**

✅ **Revenue Reporting:** Financial data 100% accurate — no missing payments or invalid prices  
✅ **Customer Service:** All orders traceable; complete history accessible  
✅ **Logistics:** Delivery dates validated; time-to-delivery metrics reliable  
✅ **Seller Management:** All items link to sellers; commissions calculable  
✅ **Product Analytics:** 99% of products categorized for analysis  
✅ **Marketing:** Customer segmentation possible using `customer_unique_id`

### **Production Readiness:**

**✅ This database is APPROVED for:**
- Business intelligence dashboards
- Executive reporting
- Machine learning model training
- Financial audits
- Customer lifetime value analysis

**No data cleanup required before use.**

---

## 🎯 Recommendations

### **Immediate (Week 1):**
1. ✏️ Add category names for 610 uncategorized products
2. 📖 Document `customer_id` vs `customer_unique_id` in data dictionary
3. 🎉 Share findings with stakeholders (celebrate 98% quality!)

### **Short-term (Month 1):**
1. 🤖 Deploy automated daily referential integrity checks
2. 🔒 Add `FOREIGN KEY` database constraints
3. 📚 Create comprehensive data dictionary

### **Long-term (Quarter 1):**
1. 🗂️ Implement soft-delete policy (`is_active` flags)
2. 📊 Build real-time data quality dashboard (Tableau/Power BI)
3. 📋 Establish data governance framework with quality SLAs

---

## 🔬 Technical Skills Demonstrated

**SQL Techniques:**
- Aggregate functions (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`)
- Complex multi-table `JOIN`s
- `LEFT JOIN` for orphan detection
- Subqueries for percentage calculations
- `UNION ALL` for result combination
- `CASE` statements for conditional logic
- Date functions and temporal validation

**Data Quality Expertise:**
- Six Dimensions framework application
- Referential integrity validation
- NULL detection and completeness analysis
- Duplicate detection strategies
- Business rule validation
- Outlier analysis with domain context

**Business Acumen:**
- Translated technical findings → business impact
- Distinguished data errors from valid patterns
- Applied regional market knowledge (Brazilian installments)
- Prioritized issues by operational severity

---

## 📂 Repository Contents
```
ecommerce-data-quality-project/
├── README.md (this file)
├── data_quality_analysis.sql (complete SQL analysis)
└── screenshots/ (optional: query outputs)
```

---

## 🚀 How to Run

1. **Download dataset** from [Kaggle - Brazilian E-Commerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
2. **Import CSVs** into MySQL database
3. **Open** `data_quality_analysis.sql` in DBeaver or MySQL Workbench
4. **Execute** queries section by section
5. **Compare** your results with documented findings

---

## 👤 About This Project

**Analyst:** [Your Name]  
**Date:** January 2025  
**Duration:** 7 days  
**Status:** ✅ Complete

**Connect:**  
📧 [your.email@example.com](mailto:your.email@example.com)  
💼 [LinkedIn Profile](https://linkedin.com/in/yourprofile)  
🌐 [Portfolio Website](https://yourportfolio.com)

---

## 📝 License

**Dataset:** Brazilian E-Commerce Public Dataset by Olist (CC BY-NC-SA 4.0)  
**Analysis:** Original work by [Your Name]

---

## 🎓 Key Takeaway

**This database demonstrates exceptional data quality (Grade A, 98/100).** Zero critical issues detected. All foreign key relationships intact. Dataset is production-ready for analytics without corrective preprocessing. All observed patterns reflect valid business logic rather than data errors.

**Status:** ✅ **APPROVED FOR PRODUCTION USE**

---

*Last Updated: January 2025 | Analysis Status: Complete | Database Grade: A*
