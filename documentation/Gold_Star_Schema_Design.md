
# Gold Layer — Star Schema Design

## Project
Microsoft Fabric | Medallion Architecture & Retail Sales Analytics

## Objective

Transform validated Silver sales transactions into a dimensional
star schema optimized for analytical reporting in Power BI.

## Data Model

The Gold layer contains one fact table and three dimension tables.

| Table | Type | Purpose |
|---|---|---|
| FactSales | Fact | Transaction-level sales measures |
| DimDate | Dimension | Calendar-based analysis |
| DimProduct | Dimension | Product-level analysis |
| DimCustomer | Dimension | Customer-level analysis |

## FactSales

Grain: One row per sales transaction.

Key columns:
- TransactionID
- DateKey
- ProductID
- CustomerID

Numeric columns:
- Quantity
- UnitPrice
- SalesAmount

## Dimension Tables

### DimDate
Contains DateKey, Date, Year, Quarter, MonthNumber,
MonthName, DayOfMonth, and DayOfWeek.

### DimProduct
Contains distinct ProductID values.

### DimCustomer
Contains distinct CustomerID values, including the
UNKNOWN customer identifier used during data cleansing.

## Relationships

| Dimension | Fact Table | Join Column |
|---|---|---|
| DimDate | FactSales | DateKey |
| DimProduct | FactSales | ProductID |
| DimCustomer | FactSales | CustomerID |

All three relationships are one-to-many, with
single-direction filtering from dimension to fact.

## Data Quality Validation

- FactSales rows: 991
- Unique transactions: 991
- Missing date relationships: 0
- Missing product relationships: 0
- Missing customer relationships: 0
- Invalid sales rows: 0

## Business Value

The dimensional model supports:
- Sales revenue analysis
- Monthly sales trends
- Product performance comparison
- Transaction and unit-volume analysis
- Interactive Power BI filtering

## Technologies

Microsoft Fabric, PySpark, Delta Lake, SQL,
Power BI Semantic Model, and DAX.
