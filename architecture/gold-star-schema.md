
# Gold Layer — Star Schema Diagram

```mermaid
erDiagram
    DimDate ||--o{ FactSales : DateKey
    DimProduct ||--o{ FactSales : ProductID
    DimCustomer ||--o{ FactSales : CustomerID

    DimDate {
        int DateKey PK
        date Date
        int Year
        int MonthNumber
        string MonthName
        int Quarter
    }

    DimProduct {
        string ProductID PK
    }

    DimCustomer {
        string CustomerID PK
    }

    FactSales {
        string TransactionID
        int DateKey FK
        string ProductID FK
        string CustomerID FK
        long Quantity
        double UnitPrice
        double SalesAmount
    }
```

## Model Description

The Gold analytical model follows a star schema design.

- **FactSales:** Transaction-level sales data.
- **DimDate:** Calendar attributes for time-based reporting.
- **DimProduct:** Distinct product identifiers.
- **DimCustomer:** Distinct customer identifiers.

Each dimension has a one-to-many relationship with FactSales.

The model supports interactive filtering and DAX-based sales analysis in Power BI.
