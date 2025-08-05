# Super Shop Sales Analysis (SQL, Power BI, Machine Learning)
This project focuses on end-to-end analysis of a synthetic yet realistic super shop sales dataset in the context of Bangladesh's retail environment. It includes: 
1) Exploratory Data Analysis (EDA) using SQL in Google BigQuery.
2) Interactive Dashboarding using Power BI.
3) Customer Segmentation & Clustering using Machine Learning.



##  Table of Contents

| Section Title                             | Link                                                   |
|-------------------------------------------|--------------------------------------------------------|
| Basic EDA in SQL                          | [View Queries and Output]()                      |
| SQL Problem Solution                      | [View Queries and Output]()                  |
| Power BI Dashboard                        | [View Queries and Output]()                  |
| Extra Insight: Clustering with ML         | [View Queries and Output]()     |


### Video Explanation [Link]()

## SQL Part

### SQL Data Quality & Exploratory Analysis Queries

###  1. Total Row Count

```sql
SELECT COUNT(*) AS TotalRows
FROM practiceproject-464611.SuperShopSales.fact_sales;
```

 **Output:**
| Row | TotalRows |
|-----|-----------|
| 1   | 75000     |

**Findings:**
- The sales table contains 75,000 transactions
- This count gives us the total volume of sales records

---

Here's the formatted analysis for your missing values check:

```markdown
### 2. Missing Values Analysis

```sql
SELECT
  SUM(CASE WHEN SaleID IS NULL THEN 1 ELSE 0 END) AS Missing_SaleID,
  SUM(CASE WHEN ProductID IS NULL THEN 1 ELSE 0 END) AS Missing_ProductID,
  SUM(CASE WHEN CustomerID IS NULL THEN 1 ELSE 0 END) AS Missing_CustomerID,
  SUM(CASE WHEN StoreID IS NULL THEN 1 ELSE 0 END) AS Missing_StoreID,
  SUM(CASE WHEN SaleDate IS NULL THEN 1 ELSE 0 END) AS Missing_SaleDate,
  SUM(CASE WHEN Quantity IS NULL THEN 1 ELSE 0 END) AS Missing_Quantity,
  SUM(CASE WHEN UnitPrice IS NULL THEN 1 ELSE 0 END) AS Missing_UnitPrice,
  SUM(CASE WHEN DiscountApplied IS NULL THEN 1 ELSE 0 END) AS Missing_Discount,
  SUM(CASE WHEN PaymentMethod IS NULL THEN 1 ELSE 0 END) AS Missing_PaymentMethod,
  SUM(CASE WHEN CampaignID IS NULL THEN 1 ELSE 0 END) AS Missing_CampaignID,
  SUM(CASE WHEN TotalAmount IS NULL THEN 1 ELSE 0 END) AS Missing_TotalAmount
FROM practiceproject-464611.SuperShopSales.fact_sales;
```

**Output:**
| Missing_SaleID | Missing_ProductID | Missing_CustomerID | Missing_StoreID | Missing_SaleDate | Missing_Quantity | Missing_UnitPrice | Missing_Discount | Missing_PaymentMethod | Missing_CampaignID | Missing_TotalAmount |
|---------------|------------------|-------------------|----------------|-----------------|-----------------|------------------|-----------------|----------------------|-------------------|--------------------|
| 0             | 0                | 773               | 0              | 0               | 0               | 0                | 12,263          | 0                     | 3,628             | 0                  |

**Findings:**
- Critical fields (SaleID, ProductID, StoreID, SaleDate, Quantity, UnitPrice, TotalAmount) have **0 missing values** ✅
- **CustomerID missing in 773 records** (1.03% of total)
- **DiscountApplied missing in 12,263 records** (16.35% of total)
- **CampaignID missing in 3,628 records** (4.84% of total)

**Data Quality Recommendations:**
1. Investigate CustomerID missingness:
   - Are these anonymous/guest purchases?
   - Check if missing values follow any pattern (specific stores/dates)
   
2. DiscountApplied missingness:
   - Verify if NULL means "no discount" (consider replacing NULLs with 0)
   - Check if discounts are properly recorded for promotional periods

3. CampaignID missingness:
   - Determine if NULL represents "no campaign" purchases
   - Consider creating a "NO_CAMPAIGN" placeholder value
