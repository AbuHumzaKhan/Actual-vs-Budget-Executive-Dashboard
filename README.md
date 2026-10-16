# Actual vs Budget — Executive Dashboard

An interactive business-intelligence project for reviewing sales against budget, identifying variance drivers, and communicating recommendations to management.

> **Data disclaimer:** This project uses synthetic FY2025 practice data. It does not represent actual company performance.

## Project contents

- [Interactive dashboard](dashboard/index.html) — standalone HTML dashboard with filters, charts, management tables, dark mode, and CSV export.
- [Summary dataset](data/actual_budget_summary.csv) — actual sales and budget summaries by product, region, sales channel, and category.
- [Data dictionary](data/data_dictionary.md) — definitions and modeling guidance.

## Business questions

1. Are sales meeting budget?
2. What is the revenue gap in INR and as a percentage?
3. Which products, regions, channels, and categories explain the largest gaps?
4. Which dimensions have the lowest budget attainment?
5. What should management investigate first?

## Quick start

Open `dashboard/index.html` in a modern browser. No build tools, server, or external libraries are required. Use the dimension/member filters to explore results, select a chart metric, and export the filtered view to CSV.

## Data model

The source workbook has two logical tables:

### `Tblactual`
Transaction grain: one row per sales transaction. Important fields include `TransactionID`, `Date`, `ProductID`, `Product`, `Category`, `Region`, `SalesChannel`, `Quantity`, `NetSalesINR`, `TotalCostINR`, and `ProfitINR`.

### `tblbudget`
Monthly target grain: one row per month, product, region, and sales channel. Important fields include `MonthStart`, `ProductID`, `Product`, `Category`, `Region`, `SalesChannel`, `BudgetSalesINR`, `BudgetQuantity`, and `BudgetProfitINR`.

**Modeling rule:** aggregate actual transactions to month × product × region × sales channel before comparing with budget. Use a calendar table and shared dimensions in Power BI. A direct join between transaction-level actuals and monthly budget rows can duplicate budget values and inflate totals.

## KPI definitions

| KPI | Definition |
|---|---|
| Actual sales | Sum of `Tblactual[NetSalesINR]` |
| Budget sales | Sum of `tblbudget[BudgetSalesINR]` |
| Sales variance | Actual sales − budget sales |
| Budget attainment | Actual sales ÷ budget sales × 100 |
| Actual profit | Sum of `Tblactual[ProfitINR]` |
| Profit margin | Actual profit ÷ actual sales × 100 |

A negative sales variance indicates an unfavourable gap. Attainment below 100% means the target was not met.

## Findings

### Executive summary

Across the synthetic FY2025 dataset, actual net sales were **₹7,04,20,415** against budget sales of **₹8,05,70,479.96**. The unfavourable variance is **₹1,01,50,064.96** (about **₹1.02 crore**), and budget attainment is **87.4%**. Sales are approximately **12.6% below plan**.

Recorded actual profit is **₹1,68,37,515**, with an approximate **23.9%** profit margin. This is based on simplified synthetic unit-cost assumptions, not an audited P&L.

### Product findings

| Product | Actual sales | Budget sales | Variance | Attainment |
|---|---:|---:|---:|---:|
| Laptop | ₹3,77,00,000 | ₹4,25,62,295.53 | -₹48,62,295.53 | 88.6% |
| Smartphone | ₹2,31,26,400 | ₹2,51,84,025.67 | -₹20,57,625.67 | 91.8% |
| Desk | ₹45,63,900 | ₹64,93,118.21 | -₹19,29,218.21 | 70.3% |
| Office Chair | ₹34,51,175 | ₹44,56,243.74 | -₹10,05,068.74 | 77.5% |
| Headphones | ₹15,78,940 | ₹18,74,796.81 | -₹2,95,856.81 | 84.2% |

**Interpretation:** Laptops have the largest absolute product gap (about ₹48.6 lakh), reflecting their revenue scale. Desks have the lowest attainment at 70.3%, signalling the weakest relative performance. Treat these as two different priorities: protect and recover high-value laptop revenue, while diagnosing why desk sales are so far below plan.

### Regional findings

| Region | Actual sales | Budget sales | Variance | Attainment |
|---|---:|---:|---:|---:|
| North | ₹1,76,98,390 | ₹1,93,81,241.34 | -₹16,82,851.34 | 91.3% |
| South | ₹1,97,34,610 | ₹2,22,52,494.93 | -₹25,17,884.93 | 88.7% |
| West | ₹1,78,10,870 | ₹2,06,04,697.22 | -₹27,93,827.22 | 86.4% |
| East | ₹1,51,76,545 | ₹1,83,32,046.47 | -₹31,55,501.47 | 82.8% |

**Interpretation:** East has the lowest attainment (82.8%) and largest regional gap (about ₹31.6 lakh). It should be prioritised for a product-by-channel diagnostic. North is closest to target at 91.3%, but is still below budget.

### Sales-channel findings

| Channel | Actual sales | Budget sales | Variance | Attainment |
|---|---:|---:|---:|---:|
| Retail | ₹2,79,01,760 | ₹3,00,58,487.52 | -₹21,56,727.52 | 92.8% |
| Online | ₹2,73,93,970 | ₹3,06,75,382.54 | -₹32,81,412.54 | 89.3% |
| Distributor | ₹1,51,24,685 | ₹1,98,36,609.90 | -₹47,11,924.90 | 76.2% |

**Interpretation:** Distributor is the clearest channel-level concern: 76.2% attainment and a gap of approximately ₹47.1 lakh. Investigate sell-through, stock availability, order fulfilment, active-account coverage, and target assumptions. The dataset identifies where the gap exists; it does not prove its cause.

### Category findings

| Category | Actual sales | Budget sales | Variance | Attainment |
|---|---:|---:|---:|---:|
| Electronics | ₹6,08,26,400 | ₹6,77,46,321.20 | -₹69,19,921.20 | 89.8% |
| Furniture | ₹80,15,075 | ₹1,09,49,361.95 | -₹29,34,286.95 | 73.2% |
| Accessories | ₹15,78,940 | ₹18,74,796.81 | -₹2,95,856.81 | 84.2% |

**Interpretation:** Electronics has the largest absolute category gap because it is the largest category by revenue. Furniture has the lowest attainment at 73.2%. Prioritisation should consider both the size of the gap and relative attainment, rather than variance alone.

### Recommendations to management

1. **Investigate distributors first.** Review order volume, sell-through, fulfilment, stock-outs, active accounts, and territory coverage versus plan.
2. **Prioritise the East region.** Break down its gap by product and channel to identify the specific combinations driving underperformance.
3. **Create a product recovery plan.** Focus on the laptop gap for absolute revenue recovery and desk attainment for relative improvement.
4. **Validate budget assumptions.** Check seasonality, planned units, price/discount assumptions, and channel target allocation.
5. **Establish a monthly variance cadence.** Track actual, budget, variance, attainment, profit, corrective action, and accountable owner.
6. **Protect margin.** Evaluate incentives and discounts against profitability, not revenue alone.

### Stakeholder presentation script

> “In this synthetic FY2025 example, we delivered ₹7.04 crore in net sales against a budget of ₹8.06 crore, leaving a ₹1.02 crore shortfall. Distributor sales are the first channel to investigate, with 76.2% attainment and a ₹47.1 lakh gap. East is the weakest region at 82.8% attainment. Furniture is the weakest category at 73.2%, while laptops create the largest absolute product gap. I recommend a focused distributor and East-region diagnostic, a targeted product recovery plan, and a monthly variance review. These are illustrative findings from synthetic data, not claims about a real company.”

## Limitations and assumptions

- All data is synthetic and intended for practice.
- The repository CSV is an **aggregated summary**, not the full transaction-level workbook.
- Budget and actual must be compared at aligned grain; never sum repeated budget rows after joining them to transaction records.
- Profit uses simplified generated cost assumptions and should not be interpreted as audited operating profit.
- Variance analysis locates the gap but does not establish root cause. Operational data is needed to validate explanations.
- Displayed percentages and currency values may differ slightly due to rounding.

## Skills demonstrated

Business analysis, KPI definition, variance analysis, aggregation-grain awareness, data modeling, interactive HTML/CSS/JavaScript, CSV export, and stakeholder communication.

## Future improvements

- Upload a new Excel workbook through the dashboard.
- Add month-level drill-down at aligned product/region/channel grain.
- Add budget-profit variance and quantity attainment.
- Add automated validation and a repeatable refresh pipeline.
- Deploy the dashboard using GitHub Pages.

## License

No license is included yet. Add one before explicitly granting others permission to reuse or redistribute the project.
