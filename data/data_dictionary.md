# Data Dictionary

## `actual_budget_summary.csv`

An aggregated summary derived from the synthetic FY2025 workbook. It contains one row per member for product, region, channel, and category, plus a total row.

| Field | Definition |
|---|---|
| `dimension_type` | Grouping type: product, region, channel, category, or total |
| `dimension_name` | Member label or total |
| `actual_sales_inr` | Actual net sales in Indian rupees |
| `budget_sales_inr` | Budget sales in Indian rupees |
| `sales_variance_inr` | Actual sales minus budget sales |
| `budget_attainment_pct` | Actual sales divided by budget sales × 100 |
| `actual_profit_inr` | Actual profit; populated on the total row only |
| `actual_profit_margin_pct` | Actual profit divided by actual sales × 100; populated on the total row only |

## Source-table grain

- **`Tblactual`:** one row per sales transaction.
- **`tblbudget`:** one row per month, product, region, and sales channel.
- Actual date field: `Date`; budget date field: `MonthStart`, the first day of the month.

## Modeling guidance

Aggregate actuals to month × product × region × sales channel before comparing with budget. In Power BI, use a calendar table and shared dimensions. A direct transaction-to-budget join can duplicate budget values.

All values are synthetic. The summary CSV is not the raw transaction workbook.
