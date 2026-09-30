# AtliQ Hardware — Sales, Finance & Forecast Analytics Dashboard

An interactive 4-page Power BI dashboard analyzing sales, financial performance, and demand-forecast accuracy for AtliQ Hardware, a computer hardware manufacturer. Built on ~1M records across 209 customers and 397 products, integrating MySQL and Excel data sources.


## Business Problem

AtliQ Hardware's leadership needed a single source of truth to answer three questions:

1. Where is revenue growing, and is that growth profitable?
2. Which markets, segments, and customers are dragging down margin?
3. How reliable is the demand forecast, and where is it creating stockout or excess-inventory risk?

The dashboard was built to replace static, disconnected spreadsheet reporting with a live, filterable view spanning Executive, Sales, Finance, and Forecast perspectives.


## Dashboard Pages

| Page | Purpose |
|---|---|
| **Executive View** | High-level KPIs (Net Sales, Gross Margin %, Net Profit %, Forecast Accuracy), revenue by segment/channel, sub-zone performance, market share trend vs. competitors |
| **Sales View** | Customer-level and market-level sales & margin analysis, channel breakdown, product/segment drill-down |
| **Finance View** | Full P&L statement from Gross Sales to Net Profit, with Dynamic financial analysis covering Net Sales, Gross Margin, Net Profit, Costs, Deductions, and YoY performance trends |
| **Forecast View** | Forecast accuracy vs. prior year, sales quantity vs. forecast quantity, and product-level risk classification (Out of Stock / Excess Inventory) |


## Tools & Technologies

* **Power BI** – Dashboard development, data modeling, and visualization
* **Power Query** – Data cleaning and transformation
* **DAX** – Measures, KPIs, calculations, and time-based analysis
* **MySQL** – Data source
* **Excel** – Data source


## Key Metrics (FY2021)

- **Net Sales**: $824M (+207% YoY)
- **Gross Margin**: 36% (-1.6% YoY)
- **Net Profit**: -7% (down from -1% LY)
- **Forecast Accuracy**: 80% (+9.9% YoY)


## Key Features

* Interactive slicers for **region, market, customer, segment, category, and product**
* Current Year vs Previous Year comparison
* Dynamic KPI and metric analysis
* Sales and Gross Margin analysis
* Profit & Loss analysis
* Forecast Accuracy tracking
* Product and customer-level analysis
* Inventory risk identification
* Drill-down analysis across business dimensions
* Interactive dashboards for management-level performance review


## Key Insights & Business Actions

| Insight (FY2021) | Business Action |
|---|---|
| Net Sales increased from $268M to $824M, but Net Profit remained negative at -$55M | Review operating expenses and cost structure to improve profitability |
| Total deductions reached $841M, reducing $1.67B Gross Sales to $824M Net Sales | Review discount, rebate, and deduction policies to reduce revenue leakage |
| India contributes 25.6% of revenue but has a -24.65% Net Profit Margin | Investigate India-specific costs and operating expenses |
| Retailers contribute 71% of Net Sales | Explore growth in Direct and Distributor channels |
| Accessories and Notebook have the highest forecast errors and are classified as Out of Stock risk | Improve demand forecasting and inventory planning |
| Storage is classified as Excess Inventory risk with a 16% forecast error | Reassess demand assumptions and inventory levels |


## Data Model & Architecture

The project uses a **Snowflake Schema** with 8 fact tables at different grains, connected through a normalized dimension layer. The main modeling challenge was combining monthly transactional data with annual cost, expense, deduction, and market-share data while maintaining consistent calculations across different report views.

### Fact Tables

| Table | Grain | Purpose |
|---|---|---|
| `fact_sales_monthly` | Customer × Product × Month | Actual monthly sales |
| `fact_forecast_monthly` | Customer × Product × Month | Monthly sales forecasts |
| `fact_actual_estimates` | Customer × Product × Month | Combined actual and forecast quantities used to create a complete FY2022 sales view |
| `post_invoice_deductions` | Customer × Fiscal Year | Discount and other deduction percentages by customer and year |
| `manufacturing_cost` | Product × Fiscal Year | Unit manufacturing cost by product and year |
| `freight_cost` | Market × Fiscal Year | Freight cost percentage by country and year |
| `operational_expenses` | Market × Fiscal Year | Operating expenses by country and year |
| `marketshare` | Category × Sub-Zone × Fiscal Year | AtliQ and competitor market share by category and region |

### Dimension Tables

The dimensions are **snowflaked rather than fully flattened**, which keeps related attributes separated into logical tables.

- `dim_date` → rolls up to a separate fiscal_year table (fiscal_month, fiscal_quarter, fiscal_year, fiscal_year_desc)
- `dim_product` → category, division, product — with category further normalized into its own table
- `dim_customer` → channel, platform, linked to dim_market
- `dim_market` → region, further snowflaked to a separate sub_zone table

<img width="1296" height="775" alt="Screenshot 2026-09-30 163642" src="https://github.com/user-attachments/assets/829e7d1a-e84f-4371-a9e8-c194266e2824" />

