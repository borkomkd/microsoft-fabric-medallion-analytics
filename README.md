# Microsoft Fabric | Medallion Architecture & Retail Sales Analytics

**End-to-End Data Engineering, Business Intelligence & Analytics Project**

## Project Overview

This project demonstrates the implementation of an end-to-end analytics solution using Microsoft Fabric, following the Medallion Architecture (Bronze, Silver, Gold).

The solution transforms raw retail sales transactions into validated analytical data, a dimensional star schema, and an interactive Power BI sales performance report.

**Project status:** Completed and validated as a hands-on learning project.

## Business Challenge

Retail businesses need accurate and consistent sales information to monitor performance, identify revenue trends, and understand product contributions.

This project addresses the technical foundations required to deliver reliable reporting through structured data ingestion, transformation, modeling, and validation.

## Solution Architecture

[View the Microsoft Fabric Solution Architecture Diagram](architecture/solution-architecture.md)

```text
Retail Sales Dataset
        |
        v
Bronze Layer (LH_Bronze)
Raw Data Ingestion
        |
        v
Silver Layer (LH_Silver)
Data Cleaning & Validation
Dataflow Gen2
        |
        v
Gold Layer (LH_Gold)
PySpark & Delta Tables
Dimensional Star Schema
        |
        v
Power BI Semantic Model
SM_Gold_SalesAnalytics
        |
        v
Power BI Report
RPT_Gold_SalesAnalytics
```

### Gold Star Schema

The analytical model contains four tables:

- **FactSales:** Sales transactions and calculated sales amounts
- **DimDate:** Calendar and time attributes
- **DimProduct:** Unique product identifiers
- **DimCustomer:** Unique customer identifiers

The dimension tables are connected to FactSales through one-to-many relationships.


### Gold Layer — Data Model Documentation

The Gold layer implements a dimensional star schema designed
for analytical reporting in Power BI.

- [Gold Star Schema Design](documentation/Gold_Star_Schema_Design.md)
- [Gold Star Schema Diagram](architecture/gold-star-schema.md)

### Gold Layer — Technical Implementation

The Gold layer was implemented using PySpark and Delta Lake
in Microsoft Fabric. The transformation converts validated
Silver transactions into analytical fact and dimension tables.

- [Gold PySpark Transformation Documentation](documentation/Gold_PySpark_Transformation.md)
- [Gold PySpark Notebook](notebooks/NB_Gold_Transformation_GitHub.ipynb)
- [Gold SQL Validation](documentation/Gold_SQL_Validation.md)

### Project Completion

Exercise 03 was completed as a hands-on Microsoft Fabric
learning project, covering Bronze, Silver, and Gold layers,
data quality validation, dimensional modeling, and
Power BI reporting.



## Technologies Used

| Technology | Purpose |
|---|---|
| Microsoft Fabric | Unified analytics platform |
| OneLake / Lakehouse | Data storage and organization |
| Dataflow Gen2 | Data cleaning and transformation |
| PySpark | Gold-layer transformations |
| Delta Lake | Analytical table storage |
| SQL Analytics Endpoint | Data validation and reconciliation |
| Power BI Semantic Model | Dimensional relationships and DAX measures |
| Power BI | Interactive business reporting |

## Data Quality and Validation

The solution implements validation checks for duplicate transactions, invalid sales values, missing identifiers, and dimensional relationships.

**Final validation results:**

| Validation Metric | Result |
|---|---:|
| Total Transactions | 991 |
| Unique Transactions | 991 |
| Null Transaction IDs | 0 |
| Invalid Sales Rows | 0 |
| Missing Date Relationships | 0 |
| Missing Product Relationships | 0 |
| Missing Customer Relationships | 0 |

Five missing customer identifiers were standardized using the `UNKNOWN` value during Silver-layer transformation.

## Business Intelligence Results

The final Power BI report provides the following validated KPIs for 2025:

| KPI | Result |
|---|---:|
| Total Sales Revenue | 96,457.89 |
| Total Transactions | 991 |
| Total Units Sold | 2,979 |
| Average Transaction Value | 97.33 |

### Report Features

- Executive KPI cards
- Monthly sales revenue trend
- Top 10 products by sales revenue
- Interactive month filtering
- SQL-to-Power BI metric reconciliation

## Project Documentation

For the detailed implementation summary, validation results, and technical reflections, see:

[Exercise 03 — Microsoft Fabric Completion Summary](documentation/Exercise_03_Microsoft_Fabric_Completion_Summary.md)

## Key Learning Outcomes

This project strengthened practical skills in:

- Designing Medallion Architecture solutions
- Implementing data transformation workflows
- Developing PySpark-based analytical tables
- Creating dimensional star schemas
- Validating data quality and business metrics using SQL
- Building Power BI semantic models and interactive reports
- Reconciling reporting results against source data

## Consultant's Reflection

A reliable business intelligence solution requires more than an attractive dashboard.

The most important outcome of this exercise was establishing consistency between transformed data, analytical relationships, SQL validation results, and Power BI reporting.

This project demonstrates the technical foundations for delivering trustworthy analytics solutions to business stakeholders.

## Scope and Limitations

This is a hands-on learning project using sample retail sales data.

The validated implementation does not represent a production deployment. Production readiness would require additional work on automated orchestration, monitoring, security, refresh reliability, and operational governance.

No financial business impact or client outcomes are claimed.

---

**Project Focus:** Data Engineering | Business Intelligence | Microsoft Fabric | Analytics Consulting

