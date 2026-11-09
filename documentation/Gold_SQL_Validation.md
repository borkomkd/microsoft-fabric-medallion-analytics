# Gold Layer — SQL Data Validation

**Project:** Microsoft Fabric | Medallion Architecture & Retail Sales Analytics  
**Exercise:** 03 — Gold Layer Implementation  
**Validation Environment:** Microsoft Fabric Lakehouse SQL Analytics Endpoint  
**Lakehouse:** `LH_Gold`  
**Status:** Completed and validated

## 1. Objective

Validate the accuracy, consistency, and analytical readiness of the Gold layer following the PySpark transformation of Silver sales transactions into a dimensional star schema.

The validation focuses on:

- Data integrity and uniqueness
- Fact table row counts
- Sales revenue and quantity calculations
- Monthly and annual revenue reconciliation
- Fact-to-dimension relationship integrity
- Consistency between SQL results and Power BI reporting

## 2. Gold Layer Data Model

The Gold layer contains four Delta tables:

| Table | Purpose |
|---|---|
| `dbo.factsales` | Sales transactions, quantities, prices, and calculated revenue |
| `dbo.dimdate` | Calendar attributes and time-based reporting |
| `dbo.dimproduct` | Product identifiers |
| `dbo.dimcustomer` | Customer identifiers |

The fact table connects to the three dimensions using `DateKey`, `ProductID`, and `CustomerID`.

For additional documentation, see:

- [Gold Star Schema Design](Gold_Star_Schema_Design.md)
- [Gold Star Schema Diagram](../architecture/gold-star-schema.md)
- [Gold PySpark Transformation](Gold_PySpark_Transformation.md)

## 3. SQL Validation Queries

Seven numbered SQL validation queries were developed for the Gold layer.

| Query | Validation purpose |
|---|---|
| `SQL_01_Gold_Data_Integrity_Validation` | Check transaction uniqueness, null identifiers, and invalid sales values |
| `SQL_02_Gold_RowCount_Validation` | Validate Gold table record counts |
| `SQL_03_Gold_SalesMetrics_Validation` | Validate calculated sales metrics |
| `SQL_04_Gold_MonthlySales_Validation` | Validate monthly sales aggregation |
| `SQL_05_Gold_Monthly_Annual_Reconciliation` | Reconcile monthly results against annual totals |
| `SQL_06_Gold_Final_FactSales_Validation` | Validate final fact table totals |
| `SQL_07_Gold_Final_Monthly_Revenue_Validation` | Validate monthly transactions and revenue |

An additional relationship-integrity check was performed to identify unmatched dimension keys.

## 4. Data Integrity Validation

The following query validates transaction uniqueness and identifies invalid records.

```sql
SELECT
    COUNT(*) AS TotalRows,
    COUNT(DISTINCT TransactionID) AS UniqueTransactions,
    SUM(CASE WHEN TransactionID IS NULL
        THEN 1 ELSE 0 END) AS NullTransactionIDs,
    SUM(CASE WHEN Quantity <= 0
        OR UnitPrice <= 0
        OR SalesAmount <= 0
        THEN 1 ELSE 0 END) AS InvalidSalesRows
FROM dbo.factsales;
```

### Validated results

| Metric | Result |
|---|---:|
| Total rows | 991 |
| Unique transactions | 991 |
| Null transaction IDs | 0 |
| Invalid sales rows | 0 |

**Conclusion:** All 991 transactions have unique, non-null identifiers and valid positive sales values.

## 5. Final Sales Metrics Validation

The following query validates the principal sales metrics.

```sql
SELECT
    COUNT(*) AS TotalTransactions,
    COUNT(DISTINCT TransactionID) AS UniqueTransactions,
    SUM(Quantity) AS TotalUnitsSold,
    CAST(SUM(SalesAmount) AS DECIMAL(18,2))
        AS TotalSalesRevenue,
    CAST(AVG(SalesAmount) AS DECIMAL(18,2))
        AS AverageTransactionValue
FROM dbo.factsales;
```

### Validated results

| Metric | Result |
|---|---:|
| Total transactions | 991 |
| Unique transactions | 991 |
| Total units sold | 2,979 |
| Total sales revenue | 96,457.89 |
| Average transaction value | 97.33 |

These values were independently confirmed in the Power BI sales performance report.

## 6. Star Schema Relationship Validation

Fact-to-dimension joins were validated against the three Gold dimension tables.

### Validated results

| Metric | Result |
|---|---:|
| Total fact rows | 991 |
| Missing date relationships | 0 |
| Missing product relationships | 0 |
| Missing customer relationships | 0 |

**Conclusion:** Every fact row matched a record in each corresponding dimension table.

This includes the handling of unknown customer identifiers during Silver-layer cleansing.

## 7. Monthly Revenue Validation

Monthly revenue and transaction counts were validated for the 2025 reporting year.

| Month | Transactions | Revenue |
|---|---:|---:|
| January | 88 | 9,069.18 |
| February | 94 | 8,423.08 |
| March | 82 | 8,281.78 |
| April | 90 | 9,134.04 |
| May | 78 | 6,697.49 |
| June | 82 | 7,943.83 |
| July | 86 | 8,720.40 |
| August | 70 | 6,245.35 |
| September | 80 | 6,620.42 |
| October | 86 | 9,089.20 |
| November | 70 | 7,855.88 |
| December | 85 | 8,377.24 |
| **Total** | **991** | **96,457.89** |

Monthly totals reconcile with the validated annual totals.

## 8. Power BI Reconciliation

The Power BI report `RPT_Gold_SalesAnalytics` was validated against the Gold SQL results.

### Annual reporting

| KPI | SQL result | Power BI result |
|---|---:|---:|
| Total Sales Revenue | 96,457.89 | 96,457.89 |
| Total Transactions | 991 | 991 |
| Total Units Sold | 2,979 | 2,979 |
| Average Transaction Value | 97.33 | 97.33 |

### Interaction testing

The Month slicer was tested using February 2025.

| KPI | February result |
|---|---:|
| Total Sales Revenue | 8,423.08 |
| Total Transactions | 94 |
| Total Units Sold | 273 |
| Average Transaction Value | 89.61 |

The slicer correctly updated the KPI cards and report visuals. Resetting the slicer restored the full-year results.

The unwanted `(Blank)` slicer entry was also resolved during final report validation.

## 9. Validation Outcome

The completed checks confirmed that:

- The Gold fact table contains 991 unique sales transactions.
- No invalid sales records were identified.
- All fact-to-dimension relationships resolved successfully.
- Sales revenue and quantities reconciled across the analytical model.
- Monthly aggregations matched the annual totals.
- Power BI KPI values matched the SQL validation results.
- Month-based filtering worked correctly.

**Final status: Exercise 03 Gold-layer SQL validation completed successfully.**

## 10. Technical and Business Value

The validation process demonstrates the importance of data quality controls in an end-to-end analytics solution.

From a technical perspective, it verifies that data transformations preserve transaction integrity and produce consistent analytical results.

From a business perspective, it provides greater confidence that management dashboards are based on accurate, reconciled sales information.

This exercise demonstrates practical experience with Microsoft Fabric, Delta Lake, PySpark, SQL validation, dimensional modeling, and Power BI reporting.

---

*This project is a hands-on learning implementation using a sample retail sales dataset. The reported results are exercise outcomes, not production business results.*
