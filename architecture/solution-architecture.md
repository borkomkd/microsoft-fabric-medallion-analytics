
# Microsoft Fabric — Solution Architecture

## Exercise 03: Medallion Architecture & Retail Sales Analytics

```mermaid
flowchart TD
    A["Retail Sales Source Dataset"]
    B["Bronze Layer<br/>LH_Bronze<br/>Raw Transactions"]
    C["Silver Layer<br/>LH_Silver<br/>Dataflow Gen2 and Data Cleansing"]
    D["Gold Layer<br/>LH_Gold<br/>PySpark and Delta Lake"]
    E["Gold Star Schema<br/>FactSales, DimDate,<br/>DimProduct, DimCustomer"]
    F["SM_Gold_SalesAnalytics<br/>Semantic Model and DAX"]
    G["RPT_Gold_SalesAnalytics<br/>Power BI Sales Dashboard"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
```

## Architecture Overview

The solution follows the Medallion Architecture pattern in Microsoft Fabric.

- **Bronze:** Stores raw retail sales transactions.
- **Silver:** Cleans and validates transactional data using Dataflow Gen2.
- **Gold:** Uses PySpark and Delta Lake to prepare a dimensional star schema.
- **Semantic Model:** Defines relationships and DAX measures.
- **Reporting:** Delivers interactive sales performance analytics through Power BI.

## Validated Results

| Metric | Result |
|---|---:|
| Valid sales transactions | 991 |
| Total sales revenue | 96,457.89 |
| Total units sold | 2,979 |
| Average transaction value | 97.33 |
| Missing dimension relationships | 0 |
| Duplicate transaction IDs | 0 |

*This is a completed hands-on learning project using a retail sales sample dataset.*
