# Retail Sales Performance Dashboard

**Power BI | DAX | Data Modeling | Retail Analytics | Business Insights**

A self-directed Power BI portfolio project exploring how a multi-branch retailer can monitor sales performance, compare locations and product categories, identify trends and turn transactional data into readable business insights.

**Dataset:** Synthetic / illustrative retail sales data — **45 Jeddah branches, June–August 2026**.  
**Project type:** Personal data analytics and visualization project; **not an official employer report**.  
**Working report:** `SalesDashboard.pbix` (the latest report supplied by the project creator).

> All dataset values, branch identifiers and dashboard conclusions should be interpreted as synthetic examples, not real company sales, employee performance or verified commercial results.

## Project Overview

Retail sales are often recorded at transaction level, while operational decisions depend on broader questions: How are sales changing over time? Which branches contribute most? What is happening across product categories? Are customer transactions or basket values changing? This project organizes a synthetic retail dataset into a Power BI dashboard designed to make those questions easier to explore.
## Dashboard Preview

The screenshots below were uploaded to this repository under the `assets/` folder.

### Full Dashboard

![Full Dashboard](assets/Full_Dashboardd.png)

### KPI Cards

![KPI Cards](assets/KPI_Cards.png)

### Charts & Insights

![Charts and Insights](assets/Charts_and_Insights.png)

### Analytical workflow

```mermaid
flowchart LR
    A["Synthetic Excel data"] --> B["Data preparation"]
    B --> C["Data model & date context"]
    C --> D["DAX measures"]
    D --> E["Interactive Power BI dashboard"]
    E --> F["Branch, category & trend insights"]
```

## Dashboard Features

The supplied Power BI report's visual configuration references the following functionality:

- KPI cards for **Total Sales**, **Total Transactions**, **Total Quantity** and **Avg. Transaction Value**.
- Sales trends and monthly selection.
- Branch comparison and product-category sales analysis.
- Dynamic text measures for **Best Branch Insight**, **Worst Branch Insight** and **Sales Trend Insight**.
- A sales-change indicator and supporting conditional-color logic.

The report's presentation combines summary KPIs with drill-down views so the active filters influence the interpretation of results.

## Key Performance Indicators

| KPI | Intended business question |
|---|---|
| Total Sales | How much sales value falls within the current selection? |
| Total Transactions | How many distinct transactions were recorded? |
| Total Quantity | How many units were sold? |
| Avg. Transaction Value | What is the average sales value per transaction? |
| Sales Change Indicator | How does the selected period compare with the relevant prior period? |
| Best / Worst Branch Insight | Which illustrative branches lead or trail within the selected context? |
| Sales Trend Insight | What does the selected sales trajectory suggest? |

For the names verified in the report's visual definitions and examples of commonly used DAX patterns, see **[KPI & DAX documentation](docs/kpis-and-dax.md)**. The sample formulas there are *illustrations*: the PBIX model's original expression bodies have not been extracted verbatim.

## Selected DAX Measures

The following examples demonstrate the logic behind the dashboard's KPI and narrative layer. **These are portfolio-friendly illustrative DAX patterns based on the report's measures, not a verbatim export of the formulas stored in `SalesDashboard.pbix`.** Verify names and model relationships before reusing them.

### Total Sales

```dax
Total Sales =
SUM ( Sales_Raw[Sales_Amount] )
```

Calculates sales in the active month, branch and category filter context.

### Total Transactions

```dax
Total Transactions =
DISTINCTCOUNT ( Sales_Raw[Transaction_ID] )
```

Counts distinct transaction IDs in the current selection.

### Total Quantity

```dax
Total Quantity =
SUM ( Sales_Raw[Quantity] )
```

Adds the units sold for the active filters.

### Avg. Transaction Value

```dax
Avg. Transaction Value =
DIVIDE ( [Total Sales], [Total Transactions], 0 )
```

Calculates average sales per transaction while handling an empty denominator.

### Best Branch Insight

```dax
Best Branch Insight =
VAR TopBranch =
    TOPN (
        1,
        ADDCOLUMNS (
            VALUES ( Branches[Branch_Name] ),
            "@Sales", [Total Sales]
        ),
        [@Sales], DESC,
        Branches[Branch_Name], ASC
    )
VAR BranchName =
    MAXX ( TopBranch, Branches[Branch_Name] )
VAR BranchSales =
    MAXX ( TopBranch, [@Sales] )
RETURN
    IF (
        ISBLANK ( BranchName ),
        "No branch data for this selection.",
        BranchName
            & " with "
            & FORMAT ( BranchSales, "#,##0" )
            & " SAR in sales."
    )
```

Illustrates a dynamic branch summary under the active filters. The branch-name tie-breaker makes ties deterministic; this formula does not establish the exact tie handling in the original report.

### Sales Trend Insight

```dax
Sales Trend Insight =
VAR CurrentSales =
    [Total Sales]
VAR PrevSales =
    CALCULATE (
        [Total Sales],
        DATEADD ( DateTable[Date], -1, MONTH )
    )
VAR DeltaPct =
    DIVIDE ( CurrentSales - PrevSales, PrevSales )
RETURN
    IF (
        ISBLANK ( PrevSales ) || PrevSales = 0,
        "No comparable previous-month sales.",
        IF (
            DeltaPct >= 0,
            "Sales increased by "
                & FORMAT ( DeltaPct, "0.0%" )
                & " vs previous month.",
            "Sales decreased by "
                & FORMAT ( ABS ( DeltaPct ), "0.0%" )
                & " vs previous month."
        )
    )
```

Illustrates a text-based month-over-month comparison. It assumes a correctly related, continuous `DateTable` and an appropriate monthly date selection.

[See the full KPI and DAX notes →](docs/kpis-and-dax.md)

## Data & Modeling

The original working dataset is an Excel workbook named `Jeddah_Retail_Sales_Synthetic_3M.xlsx`. Its model comprises transaction data and descriptive tables for branches, products and targets. The report uses a date table for time-based analysis. The report also references a `Branch Performance` table for some analytical text and KPI visuals.

| Data component | Purpose |
|---|---|
| `Sales_Raw` | Transaction-level sales, quantities, prices, discounts and transaction IDs |
| `Branches` | Illustrative branch identifiers and descriptions |
| `Products` | Product attributes and categories |
| `Targets` | Synthetic monthly branch targets |
| `DateTable` | Calendar context for month selection and comparisons |
| `Branch Performance` | Referenced in report visuals for some KPIs and insights |

**[Data model and source notes →](docs/data-model.md)**

## Dashboard Design & Business Insights

The report is designed around four layers:

1. **Headline performance:** sales, transactions, units and average transaction value.
2. **Time:** monthly sales trends and changes under the selected date context.
3. **Composition:** sales contribution by product category and branch.
4. **Narrative:** DAX-driven insight text about branch performance and sales direction.

These outputs demonstrate an analytical workflow. They do **not** establish actual branch performance or realized revenue improvements.

**[Dashboard overview →](docs/dashboard-overview.md)**

## Skills Demonstrated

**Power BI · DAX · Data Modeling · Business Intelligence · KPI Design · Sales Analysis · Data Visualization · Interactive Filtering · Business Insight Communication**

## Repository Contents

| Resource | What it covers |
|---|---|
| [Dashboard overview](docs/dashboard-overview.md) | Business question, analysis and report structure |
| [Data model](docs/data-model.md) | Synthetic source, table roles and conceptual relationships |
| [KPI and DAX documentation](docs/kpis-and-dax.md) | Measures referenced by visuals and labeled example DAX |

### Power BI file and dashboard screenshots

Three dashboard screenshots are now included in [`assets/`](assets/) and displayed in the preview above. The working PBIX and Excel source files are **not yet attached to this repository**. Screenshots illustrate the portfolio dashboard; they should not be treated as proof that the exact latest `SalesDashboard.pbix` binary is downloadable here.

## Privacy & Data Provenance

This dashboard is a personal portfolio exercise based on synthetic data inspired by a retail operating context. It is neither an employer-endorsed publication nor a disclosure of actual business performance. Do not substitute real customer information, production sales, confidential branch performance, internal system paths or proprietary logos for its public assets.
