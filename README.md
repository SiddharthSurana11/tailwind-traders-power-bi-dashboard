# Tailwind Traders — Global Sales & Profit Intelligence Dashboard

A Power BI dashboard analyzing global retail performance for Tailwind Traders across five markets (UK, USA, Australia, France, UAE) — covering sales, profitability, product performance, and multi-currency revenue reporting.

![Sales Overview](https://github.com/SiddharthSurana11/tailwind-traders-power-bi-dashboard/blob/main/Sales%20Overview.png)
![Profit Overview](https://github.com/SiddharthSurana11/tailwind-traders-power-bi-dashboard/blob/main/Profit%20Overview.png)

## Business Problem

Tailwind Traders needed a way to answer two questions its raw transaction data couldn't answer on its own:
1. Which products, countries, and time periods are actually driving revenue — and is that revenue turning into profit?
2. Because the company sells across multiple currencies, how do you compare performance on a like-for-like basis without manually converting every transaction?

## What This Dashboard Does

- **Sales Overview** — loyalty point distribution by country, quantity sold by product, median sales trend over time, and sales distribution by country
- **Profit Overview** — year-to-date profit margin, net revenue, profit margin by country, product-level net revenue ranking, and a quarter-by-country profit ribbon chart showing how each market's profit contribution shifts over time
- **Multi-currency modeling** — a dedicated USD conversion table so every metric can be compared apples-to-apples regardless of the transaction's original currency

## Approach

**Data preparation (Power Query)**
- Cleaned and typed Sales, Purchases, and Countries datasets; validated data quality (nulls, duplicates, range checks) column by column before modeling
- Filtered Purchases to exclude returned items so profit calculations reflect only completed sales
- Pulled historical currency exchange rates via a Python script connector

**Data modeling**
- Built a Calendar table for time intelligence
- Created a calculated **Sales in USD** table with a 1:1 bidirectional relationship to the Sales table, so every transaction has both its native-currency and USD-normalized values available side by side

**Key DAX measures**

```dax
Yearly Profit Margin =
DIVIDE(
    SUM('Sales in USD'[Profit USD]),
    SUM('Sales in USD'[Net Revenue USD])
)
```
Profit as a percentage of net revenue — the core efficiency metric used across every page.

```
Gross Revenue   = Quantity Purchased × Gross Product Price
Total Tax       = Gross Revenue × Tax Rate
Net Revenue     = Gross Revenue − Total Tax
Profit          = Net Revenue − (Cost Per Unit × Quantity Purchased)
```
Base financial columns computed at the transaction level, then aggregated in every DAX measure above.

- **Quarterly Profit Margin** — the yearly margin logic applied through time-intelligence functions to surface seasonal trends quarter over quarter
- **Year-to-Date Profit Margin** — a running profit efficiency figure from the start of the fiscal year to any given date
- **Median Sales** — the statistical median of gross revenue, used instead of the mean so a handful of outlier transactions don't distort the "typical sale" figure

## Tools Used

Power BI Desktop · Power Query · DAX · Python (currency data connector) · Excel (initial data prep)

## Key Insights

- The UK leads on loyalty engagement (315 points) and holds the largest share of median sales (45%), with the USA close behind
- The Modular Sofa Set is the top revenue-generating product at $928, more than double several mid-tier products in the catalog
- Profit margin holds steady around 62% for most of the year, with a notable dip to ~34% in one period — worth flagging to the business as an anomaly to investigate
- Profit contribution by country shifts across quarters, visible in the ribbon chart's crossover pattern rather than a static ranking

## Repo Structure

```
├── Tailwind_Traders_Report.pbix     # Full Power BI report 
│── sales-overview.png
│── profit-overview.png
└── README.md
```

## How to View

Download `Tailwind_Traders_Report.pbix` and open in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free) to interact with the filters, drill-throughs, and tooltips directly.

---
Built by [Siddharth Surana](https://github.com/SiddharthSurana11)
