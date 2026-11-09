
# Microsoft Fabric — Solution Architecture

## Exercise 03: Medallion Architecture & Retail Sales Analytics

```mermaid

flowchart TB
    subgraph ROW1["Data Engineering — Medallion Architecture"]
        direction LR
        A["Retail Sales<br/>Source Dataset"]
        B["Bronze Layer<br/>LH_Bronze<br/>Raw Transactions"]
        C["Silver Layer<br/>LH_Silver<br/>Dataflow Gen2<br/>Data Cleansing"]
        D["Gold Layer<br/>LH_Gold<br/>PySpark & Delta Lake"]

        A --> B --> C --> D
    end

    subgraph ROW2["Analytics & Reporting"]
        direction LR
        E["Gold Star Schema<br/>FactSales · DimDate<br/>DimProduct · DimCustomer"]
        F["SM_Gold_SalesAnalytics<br/>Semantic Model & DAX"]
        G["RPT_Gold_SalesAnalytics<br/>Power BI Sales Dashboard"]

        E --> F --> G
    end

    D --> E

    classDef source fill:#F1F5F9,stroke:#64748B,color:#0F172A
    classDef bronze fill:#FFF1E6,stroke:#C77836,color:#713F12
    classDef silver fill:#F1F5F9,stroke:#64748B,color:#334155
    classDef gold fill:#FEF3C7,stroke:#D4A017,color:#78350F
    classDef reporting fill:#EDE9FE,stroke:#7C3AED,color:#4C1D95

    class A source
    class B bronze
    class C silver
    class D,E gold
    class F,G reporting

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
