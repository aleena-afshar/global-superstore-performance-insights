# Global Superstore Performance Insights (Power BI + Excel)

A 4-page interactive Power BI report analyzing 51,290 global orders across sales, product profitability, customer/market performance, and shipping operations — built on a dedicated Date table and a full library of DAX measures.

## Table of Contents
- [Overview](#overview)
- [Business Problem](#business-problem)
- [Dataset](#dataset)
- [Tools & Technologies](#tools--technologies)
- [Project Structure](#project-structure)
- [Data Model & DAX Measures](#data-model--dax-measures)
- [Key Metrics at a Glance](#key-metrics-at-a-glance)
- [Report Pages & Key Findings](#report-pages--key-findings)
- [How to Run This Project](#how-to-run-this-project)
- [Results & Recommendations](#results--recommendations)
- [Future Work](#future-work)
- [Author & Contact](#author--contact)

## Overview
This project turns Global Superstore's 2012–2015 order history into a 4-page executive Power BI report — Executive Overview, Product & Profitability, Customer & Market, and Shipping & Operations — connected by synced slicers (Year, Market, Segment, Category) so any filter applied on one page carries across all four. Every visual is backed by a dedicated `_Measures` table of DAX formulas rather than raw column drags, and a proper Date table drives the time-intelligence calculations (YoY growth, YTD).

## Business Problem
Global Superstore operates across five markets and dozens of countries, and leadership needs a single report that answers four distinct questions without switching tools: How is the business performing overall? Where is profitability strong or weak by product? Who are the most valuable customers and markets? And how efficient is the shipping operation? This report was built to answer all four from one connected data model.

## Dataset
- **Source:** Global Superstore 2016 dataset (`data/global_superstore_dataset.xlsx`)
- **Sheets:** `Orders`, `Returns`, `People`
- **Records:** 51,290 individual order-line items
- **Coverage:** 17,415 unique customers · 5 markets (Asia Pacific, Europe, USCA, LATAM, Africa) · 3 categories
- **Timeframe:** 2012–2015

## Tools & Technologies
- **Power BI Desktop** — data modeling, DAX measures, 4-page interactive report
- **Excel (.xlsx)** — source data format

## Project Structure
```
global-superstore-performance-insights/
├── data/
│   └── global_superstore_dataset.xlsx        # Raw source data (Orders, Returns, People)
├── dashboards/
│   └── global_superstore_dashboard.pbix       # Power BI report file
├── images/
│   ├── dashboard_executive_overview.png       # Page 1: Executive Overview
│   ├── dashboard_product_profitability.png    # Page 2: Product & Profitability
│   ├── dashboard_customer_market.png           # Page 3: Customer & Market
│   └── dashboard_shipping_operations.png       # Page 4: Shipping & Operations
├── docs/
│   └── power_bi_build_guide.pdf                # Full build guide: data model, DAX, and visual specs
├── README.md
└── .gitignore
```

## Data Model & DAX Measures
A dedicated Date table was built with `CALENDAR()` and marked as the official date table to support time intelligence, related to `Orders[Order Date]`. All measures live in a separate `_Measures` table rather than being scattered across the data table.

**Executive measures**
| Measure | DAX | Purpose |
|---|---|---|
| Total Sales | `SUM(Orders[Sales])` | Revenue |
| Total Profit | `SUM(Orders[Profit])` | Profit |
| Total Quantity | `SUM(Orders[Quantity])` | Units sold |
| Total Orders | `DISTINCTCOUNT(Orders[Order ID])` | Order count |
| Total Customers | `DISTINCTCOUNT(Orders[Customer ID])` | Customer count |
| Profit Margin % | `DIVIDE([Total Profit], [Total Sales], 0)` | Profit efficiency |
| Avg Order Value | `DIVIDE([Total Sales], [Total Orders], 0)` | Average order size |

**Time intelligence** (requires the Date table relationship)
| Measure | Purpose |
|---|---|
| Sales PY | Prior-year sales via `SAMEPERIODLASTYEAR` |
| Sales YoY % | Year-over-year growth |
| Sales YTD | Year-to-date performance via `TOTALYTD` |

**Discount & shipping**
| Measure | Purpose |
|---|---|
| Avg Discount % | Average discount applied |
| Shipping Cost % of Sales | Shipping cost as a share of revenue |
| Avg Shipping Delay (Days) | Average days between order and ship date |

## Key Metrics at a Glance
| Metric | Value |
|---|---|
| Total Sales | 12.64M |
| Total Profit | 1.47M |
| Overall Profit Margin | 12% |
| Total Orders | 26K |
| Total Customers | 17K |
| Average Order Value | 491.39 |

## Report Pages & Key Findings

### Page 1 — Executive Overview
*Business question: how is the business performing overall?*

![Executive Overview](images/dashboard_executive_overview.png)

- Sales climbed every year from 2.26M (2012) to 4.30M (2015) — nearly doubling — while profit grew in step, from 249K to 504K, keeping margin roughly stable rather than eroding as revenue scaled.
- **Asia Pacific is the strongest market (4.0M in sales)**, followed by Europe (3.3M), with **Africa trailing all markets by a wide margin (0.8M)** — under a fifth of Asia Pacific's volume.
- Technology (37.5%), Furniture (32.5%), and Office Supplies (30.0%) are close to evenly split by category — no single category dominates sales the way Asia Pacific dominates by market.

### Page 2 — Product & Profitability
*Business question: where are we making money, and where might profitability be under pressure?*

![Product & Profitability](images/dashboard_product_profitability.png)

- Technology leads category sales (4.7M), narrowly ahead of Furniture (4.1M) and Office Supplies (3.8M).
- The Canon imageCLASS 2200 Advanced Copier is the single best-selling product by revenue (62K), well ahead of the next-highest items.
- **The Region × Category profit margin matrix reveals the sharpest finding in the whole report: Central Asia posts negative margins across all three categories** — Furniture (-0.55), Office Supplies (-0.36), Technology (-0.32) — the only region in the dataset with universally negative profitability. Central US is also weak on Furniture specifically (-0.02).
- The shipping-cost-vs-profit scatter shows most products cluster in a tight, low-shipping-cost band, with a small number of high-shipping-cost outliers pulling the top-left of the chart — worth a closer look at those specific SKUs.

### Page 3 — Customer & Market
*Business question: who contributes to the business, and where are the valuable customers located?*

![Customer & Market](images/dashboard_customer_market.png)

- Consumer is the largest segment by sales (6.51M, 51.5%), more than Corporate (3.82M) and Home Office (2.31M) combined.
- Tom Ashbrook is the single highest-value customer by sales (40K), followed closely by Tamara Chand (37K) and Greg Tran (36K) — the top 10 customers span a fairly narrow 30K–40K band rather than one dominant outlier.
- The Region × Category sales matrix confirms the same geographic pattern from Page 1 and 2: Central America, Eastern Asia, and Southeast Asia are meaningful contributors, while South America and Central Asia remain the smallest.

### Page 4 — Shipping & Operations
*Business question: how efficiently are orders being shipped?*

![Shipping & Operations](images/dashboard_shipping_operations.png)

- **Same Day shipping costs the most relative to sales (17.4%)**, followed closely by First Class (16.9%) — both far more expensive proportionally than Standard Class (8.2%), even though Standard Class carries the most total sales volume (7.4M of the 12.64M total).
- Medium priority dominates order volume (14.7K orders), nearly double High priority (7.8K), with Critical (2.6K) and Low (1.2K) far behind.
- **Africa has the highest shipping cost percentage of any market (11.3%)**, ahead of LATAM (10.9%) and Asia Pacific (10.8%) — meaning the market with the lowest total sales (Page 1) also carries the least efficient shipping economics, a compounding disadvantage worth flagging to operations.

## How to Run This Project
1. Clone the repository
   ```bash
   git clone https://github.com/aleena-afshar/global-superstore-performance-insights.git
   ```
2. Open `dashboards/global_superstore_dashboard.pbix` in Power BI Desktop to explore the full interactive report — all four pages share synced Year, Market, Segment, and Category slicers.
3. Reference `data/global_superstore_dataset.xlsx` for the raw source data, or `docs/power_bi_build_guide.pdf` for the complete step-by-step build documentation (data model, every DAX measure, and every visual's exact configuration).

## Results & Recommendations
1. **Investigate Central Asia immediately** — it's the only region with negative profit margins across all three categories, which points to a systemic pricing, cost, or discount problem specific to that market rather than a single weak product line.
2. **Reassess Africa's shipping economics** — it already has the lowest total sales of any market and simultaneously the highest shipping cost as a percentage of sales, compounding an existing underperformance.
3. **Audit premium shipping pricing** — Same Day and First Class carry shipping costs 2x higher (as a % of sales) than Standard Class; confirm this reflects deliberate premium pricing rather than an unpriced cost leak.
4. **Consumer segment is the core of the business (51.5% of sales)** — retention and loyalty programs targeted at this segment carry more leverage than any other single lever in the dataset.

## Future Work
- Build a Returns-adjusted profit view using the `Returns` sheet, since current profit figures don't account for returned orders.
- Add a discount-tier breakdown to quantify how much of Central Asia's negative margin is discount-driven versus cost-driven.
- Extend the Date table with a rolling 12-month view to smooth out the sharp end-of-2015 jump visible in the Sales & Profit trend line.
- Add drill-through from the Region × Category matrix directly to an order-level detail page for root-cause investigation.

## Author & Contact
**Aleena Afshar**
Data Analyst
📧 afsharaleena@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/aleena-afshar) • [GitHub](https://github.com/aleena-afshar)
