# Gold Layer — PySpark Transformation

## Project

**Microsoft Fabric | Medallion Architecture & Retail Sales Analytics**

**Exercise 03 — Gold Layer Implementation**

## Objective

Transform validated Silver sales transactions into a dimensional star schema using PySpark and Delta Lake in Microsoft Fabric.

The Gold layer provides structured analytical tables for Power BI reporting and business performance analysis.

## Source Data

**Source Lakehouse:** `LH_Silver`  
**Source Table:** `dbo.Silver_SalesTransactions`  
**Validated Transactions:** 991

The source dataset contains:

| Column | Description |
|---|---|
| TransactionID | Unique sales transaction identifier |
| TransactionDate | Date of transaction |
| ProductID | Product identifier |
| CustomerID | Customer identifier |
| Quantity | Number of units sold |
| UnitPrice | Price per unit |

The Silver layer was validated before Gold transformations.

## Transformation Workflow

### 1. Read Silver Transactions

The notebook reads the validated Silver table into a Spark DataFrame using `spark.read.table()`.

Initial checks include transaction counts, schema inspection, and sample record review.

### 2. Create DimProduct

The product dimension is generated from distinct `ProductID` values in the Silver dataset.

**Validated result:** 31 distinct products.

The dimension contains one record per unique product identifier.

### 3. Create DimCustomer

The customer dimension is generated from distinct `CustomerID` values.

The Silver transformation replaces missing customer identifiers with `UNKNOWN`, allowing these transactions to be retained for analytical reporting.

### 4. Create DimDate

The date dimension provides calendar attributes for time-based analysis.

Key attributes include:

- DateKey
- Date
- Year
- Quarter
- MonthNumber
- MonthName
- DayOfMonth
- DayOfWeek

The dimension supports monthly sales trends and time-based filtering in Power BI.

### 5. Create FactSales

The sales fact table is generated from the validated Silver transactions.

**Fact table grain:** One row per sales transaction.

Important fields include:

| Column | Purpose |
|---|---|
| TransactionID | Transaction identifier |
| DateKey | Relationship to DimDate |
| ProductID | Relationship to DimProduct |
| CustomerID | Relationship to DimCustomer |
| Quantity | Units sold |
| UnitPrice | Price per unit |
| SalesAmount | Calculated sales revenue |

Sales revenue is calculated using:

`SalesAmount = Quantity * UnitPrice`

### 6. Save Gold Tables

The notebook writes the transformed DataFrames as Delta tables in `LH_Gold`.

The resulting star schema consists of:

- `dbo.factsales`
- `dbo.dimdate`
- `dbo.dimproduct`
- `dbo.dimcustomer`

## Data Quality and Validation

The completed Gold layer was validated using SQL queries against the Gold Lakehouse SQL analytics endpoint.

| Validation metric | Result |
|---|---:|
| Total transactions | 991 |
| Unique transactions | 991 |
| Total units sold | 2,979 |
| Total sales revenue | 96,457.89 |
| Average transaction value | 97.33 |
| Missing date relationships | 0 |
| Missing product relationships | 0 |
| Missing customer relationships | 0 |
| Invalid sales rows | 0 |

The results demonstrate consistency between the Gold analytical tables and the Power BI reporting metrics.

## PySpark Notebook

[View the Gold Transformation Notebook](../notebooks/NB_Gold_Transformation_GitHub.ipynb)

The notebook contains the PySpark transformation logic used to construct the Gold layer.

The GitHub version excludes saved execution outputs and is intended for technical review. Execution requires an appropriately configured Microsoft Fabric environment and access to the source Lakehouse.

## Architecture References

- [Solution Architecture](../architecture/solution-architecture.md)
- [Gold Star Schema Design](Gold_Star_Schema_Design.md)
- [Gold Star Schema Diagram](../architecture/gold-star-schema.md)

## Technical Outcome

The implementation demonstrates the ability to:

- Transform analytical datasets using PySpark.
- Build dimensional models for business intelligence.
- Persist analytical data using Delta Lake.
- Validate data integrity using SQL.
- Connect structured analytical tables to Power BI.
- Document a reproducible data engineering workflow.

**Project status:** Completed and validated as a hands-on Microsoft Fabric learning project.
