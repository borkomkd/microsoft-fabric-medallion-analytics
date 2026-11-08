# Exercise 03 — Microsoft Fabric Medallion Architecture & Gold Sales Analytics

**Status:** Completed and validated for the exercise scope  
**Environment:** Microsoft Fabric — `Workspace_3`  
**Deliverable:** `RPT_Gold_SalesAnalytics`  
**Dataset period:** 2025

## Executive summary

This hands-on exercise implements a retail sales analytics solution in Microsoft Fabric, progressing from raw transactions through a Bronze–Silver–Gold architecture to a Power BI report. The work emphasizes traceable transformations, a dimensional Gold layer, and reconciliation between SQL results and report metrics.

The solution is a learning demonstration using exercise data. It is **not** presented as a production deployment or evidence of realized business impact.

## Business problem

Retail decision-makers need consistent answers to questions such as:

- How much sales revenue did the business generate?
- How many transactions and units were sold?
- How does revenue change month by month?
- Which products contribute the most revenue?

Reliable reporting requires data quality checks and consistent definitions, not only charts.

## Solution architecture

```text
Retail sales source
       |
       v
LH_Bronze  — raw sales data
       |
       v
LH_Silver  — DF_Silver_Transformation (Dataflow Gen2)
             validate and clean transactions
       |
       v
LH_Gold    — NB_Gold_Transformation (PySpark / Delta)
             ├── factSales
             ├── dimDate
             ├── dimProduct
             └── dimCustomer
       |
       v
SM_Gold_SalesAnalytics — semantic model and DAX measures
       |
       v
RPT_Gold_SalesAnalytics — interactive Power BI report
```

### Components

| Layer | Artifact | Purpose |
|---|---|---|
| Bronze | `LH_Bronze` | Store raw ingested transactions |
| Silver | `LH_Silver` | Store validated and cleaned sales transactions |
| Silver | `DF_Silver_Transformation` | Apply data quality transformations using Dataflow Gen2 |
| Gold | `LH_Gold` | Store analytical fact and dimension tables |
| Gold | `NB_Gold_Transformation` | Create Gold tables with PySpark |
| Semantic | `SM_Gold_SalesAnalytics` | Define relationships and reusable measures |
| Reporting | `RPT_Gold_SalesAnalytics` | Present KPIs, monthly trends, and product rankings |

## Data preparation and modeling

The Silver transformation process:

1. Removed rows with invalid quantities or unit prices.
2. Removed duplicate transactions.
3. Replaced missing `CustomerID` values with `UNKNOWN` (five records).
4. Produced **991 validated transactions**.

The Gold layer organizes data as a star schema:

- **`factSales`**: transaction-level facts and sales amounts.
- **`dimDate`**: dates, months, and calendar attributes for time analysis.
- **`dimProduct`**: distinct product identifiers; descriptive product metadata was not available in the exercise source.
- **`dimCustomer`**: customer identifiers, including handling for unknown customers.

The semantic model connects the dimensions to the fact table and defines reusable reporting measures.

## Final validated KPIs

| Metric | Full-year 2025 result |
|---|---:|
| Total Sales Revenue | 96,457.89 |
| Total Transactions | 991 |
| Total Units Sold | 2,979 |
| Average Transaction Value | 97.33 |

The average transaction value is total sales revenue divided by the number of transactions, rounded to two decimal places.

## Validation evidence

### Data integrity

| Check | Result |
|---|---:|
| Total fact rows | 991 |
| Distinct TransactionIDs | 991 |
| Null TransactionIDs | 0 |
| Invalid sales rows | 0 |

### Dimension relationship checks

| Check | Result |
|---|---:|
| Missing date matches | 0 |
| Missing product matches | 0 |
| Missing customer matches | 0 |

### Reporting reconciliation

- Monthly revenue was validated using SQL for January–December 2025.
- Full-year report KPIs matched the Gold SQL validation results.
- The month slicer was tested using February 2025:

| February KPI | Validated report value |
|---|---:|
| Total Sales Revenue | 8,423.08 |
| Total Transactions | 94 |
| Total Units Sold | 273 |
| Average Transaction Value | 89.61 |

- Resetting the slicer to **All** restored all four validated full-year totals.
- The `(Blank)` month option was excluded from the slicer after checking date relationships and nulls; the source-level tests showed no null month names, unmatched fact dates, or null fact date keys.

### Saved SQL validation queries

The final SQL query collection includes:

- `SQL_01_Gold_Data_Integrity_Validation`
- `SQL_02_Gold_RowCount_Validation`
- `SQL_03_Gold_SalesMetrics_Validation`
- `SQL_04_Gold_MonthlySales_Validation`
- `SQL_05_Gold_Monthly_Annual_Reconciliation`
- `SQL_06_Gold_Final_FactSales_Validation`
- `SQL_07_Gold_Final_Monthly_Revenue_Validation`

> **Naming note:** A relationship-validation query was also run successfully and previously discussed under the name `SQL_06_Gold_Final_Relationship_Validation`. The final screenshot of the saved query list instead shows `SQL_06_Gold_Final_FactSales_Validation`. Verify the exact saved name in Fabric before treating the relationship-validation query as a separate repository artifact.

## Power BI report

**Report:** `RPT_Gold_SalesAnalytics`  
**Semantic model:** `SM_Gold_SalesAnalytics`

The report contains:

- Four KPI cards: revenue, transactions, units sold, and average transaction value.
- Monthly Sales Revenue Trend (January–December 2025).
- Top 10 Products by Sales Revenue.
- Interactive month selector.

Formatting was verified, including the revenue thousands separator and two decimal places. The report was checked in viewing mode.

### Screenshots to add to the repository

Add your own Fabric screenshots to an `images/` directory, then uncomment the links below:

```markdown
<!-- ![Bronze–Silver–Gold Lakehouses](images/medallion-lakehouses.png) -->
<!-- ![Gold star schema](images/gold-star-schema.png) -->
<!-- ![Gold SQL validation results](images/gold-sql-validation.png) -->
<!-- ![Power BI sales overview](images/power-bi-sales-overview.png) -->
```

## Skills demonstrated

- Microsoft Fabric Lakehouse organization and Medallion Architecture.
- Dataflow Gen2 transformation and data quality controls.
- PySpark transformations and Delta tables.
- Fact and dimension modeling for analytics.
- SQL analytics endpoint validation and reconciliation.
- Semantic model relationships and DAX measures.
- Interactive Power BI report development.
- End-to-end validation of business metrics.

## Limitations and next improvements

This exercise demonstrates a working analytical solution, but does not establish production readiness. Potential next steps include:

1. Orchestrate and schedule the pipeline, with retries and failure notifications.
2. Add data quality thresholds, monitoring, and operational logs.
3. Implement incremental processing and test refresh reliability.
4. Review access controls, data governance, and deployment processes.
5. Enrich product and customer dimensions with meaningful business attributes when source data is available.
6. Capture screenshots and reproducible notebook/SQL scripts for GitHub.

## Consultant's reflection

The key lesson is that business-facing dashboards are only as trustworthy as the data and calculations behind them. Building the layers, validating relationships, and reconciling SQL totals with Power BI metrics creates a stronger foundation for decision-making than reporting alone.

**Key takeaway:** An end-to-end analytics solution should demonstrate not just visual outputs, but also verifiable data quality and consistent business definitions.

## Portfolio positioning

This exercise is best presented as a **technical learning project** showing hands-on Microsoft Fabric capabilities. A future portfolio flagship case study can build on these skills by adding a clearly defined business challenge, stakeholder requirements, analytical recommendations, and quantified impact only where supported by evidence.

---

*Prepared from the completed Exercise 03 workflow and validation screenshots. Replace illustrative descriptions with repository links and original screenshots before public publication.*
