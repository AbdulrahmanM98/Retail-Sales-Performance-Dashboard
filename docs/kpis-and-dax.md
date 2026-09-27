# KPIs and DAX documentation

The supplied `SalesDashboard.pbix` report's visual definitions reference the following measures. The full DAX expression bodies were **not independently extracted from the compressed Power BI model**. The code snippets below are therefore **illustrative definitions**, not exact copies of the original measures.

## Measures referenced by the report visuals

| Report-referenced measure | Business meaning |
|---|---|
| `Total Sales` | Sales amount in the active filter context |
| `Total Transactions` | Transaction count |
| `Total Quantity` | Units sold |
| `Avg. Transaction Value` | Average sales per transaction |
| `Best Branch Insight` | Dynamic high-performing branch summary |
| `Worst Branch Insight` | Dynamic low-performing branch summary |
| `Sales Trend Insight` | Dynamic narrative about sales over time |
| `Sales Change Indicator` | Change against the comparative period |

The report also contains visual references to `Sales Change Color`. Additional change indicators or color measures may exist in the model but should not be asserted as verified solely from the visible visual configuration.

## Illustrative, reproducible DAX patterns

```dax
Total Sales =
SUM ( Sales_Raw[Sales_Amount] )

Total Transactions =
DISTINCTCOUNT ( Sales_Raw[Transaction_ID] )

Total Quantity =
SUM ( Sales_Raw[Quantity] )

Avg. Transaction Value =
DIVIDE ( [Total Sales], [Total Transactions], 0 )
```

The transaction count assumes `Transaction_ID` is the intended transaction-level identifier; verify this against the model's data grain and actual saved measure.

A sales target achievement measure, **if present in the finished Power BI model**, can be approached as:

```dax
Target Achievement % =
DIVIDE ( [Total Sales], [Sales Target], 0 )
```

The example above requires a separately defined `[Sales Target]` measure and a target table with the correct calendar and branch filter context. It is a suggested pattern, **not proof the named measure appears in the supplied report**.

## Insights and filter context

The interesting aspect of this dashboard is that numeric KPIs are accompanied by branch and trend narratives. An insight measure should honor month and branch slicers rather than always reporting all-time totals.

Dynamic narratives should distinguish the active selection, the comparison period and the unit of the metric. Avoid interpreting synthetic values as evidence of real business performance.
