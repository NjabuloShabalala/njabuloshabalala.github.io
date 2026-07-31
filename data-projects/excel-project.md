# Hi-Fi Org Sales Performance Dashboard

## Key Results

| Total Revenue | Total Orders | Avg. Order Value | Top Region |
|:---:|:---:|:---:|:---:|
| **R2,452,084.30** | **2,000** | **R1,226.04** | **North** |

---

## The Problem

Hi-Fi Org's sales activity was captured as raw, transaction-level data — every order, product, region, and channel logged as a flat row with no way to see the bigger picture. Leadership had no quick way to answer basic questions: which regions were actually performing, which product categories were driving revenue, or how sales were splitting across online versus in-store channels. Without a way to slice this data on demand, any answer meant manually filtering and re-summing thousands of rows every time a new question came up.

## What I Built

An interactive Excel dashboard built on top of the raw transaction data, using pivot tables as the underlying engine and slicers (Product, Product Category, Region, Sales Channel, and Month/Quarter/Year date hierarchies) to let anyone filter the entire report in real time — no formulas to edit, no re-querying required.

The dashboard answers five core business questions, each mapped directly from a stakeholder requirement to a specific chart and underlying data column:

| Dashboard Requirement | Dashboard Element | Column Used |
|---|---|---|
| Total Revenue | Total Revenue KPI | Revenue |
| Total Orders | Total Orders KPI | Order ID |
| Quantity Sold | Total Quantity Sold KPI | Quantity |
| Average Order Value | Average Order Value KPI | Revenue |
| Monthly Revenue Trend | Revenue by Month | Date and Revenue |
| Regional Performance | Revenue by Region | Region and Revenue |
| Product Category Performance | Revenue by Product Category | Product Category and Revenue |
| Sales Channel Contribution | Revenue by Sales Channel | Sales Channel and Revenue |
| Top Products by Revenue | Top 5 Products by Revenue | Product and Revenue |

---

## Dashboard Showcase

![Hi-Fi Org Dashboard](/assets/hi-fi-org-dashboard.png)
*Figure 1: Full interactive dashboard view — KPI summary, monthly revenue trend, regional and category breakdowns, sales channel split, and top 5 products by revenue, all filterable via slicers.*

---

## Key Insights

**1. Regional Performance**
North (R561,753.72) narrowly edged out Central (R556,705.18) as the top-earning region, with South (R444,494.30) and West (R421,322.25) trailing. The spread between the top and bottom region is relatively tight, suggesting revenue is fairly evenly distributed geographically rather than concentrated in a single territory.

**2. Product Category Concentration**
Computers dominated category revenue at R1,346,491.26 — more than double the next closest category, Monitors, at R526,744.59. Networking, Storage, Accessories, and Office Supplies made up the remainder, with Office Supplies contributing the least at R16,640.26. This signals a business that is heavily reliant on a single high-value category, which carries some concentration risk worth flagging to leadership.

**3. Sales Channel Mix**
Online sales led as the largest single channel, followed by Corporate Sales, Retail Store, and Marketplace. This suggests digital demand generation and direct B2B relationships are currently outperforming both physical retail and third-party marketplace listings.

**4. Top Product Performance**
The Gaming Laptop was the single highest-revenue product at R432,576.06 — well ahead of the next best performer, Laptop Air 13, at R273,532.50. High-ticket computing hardware (laptops in particular) clearly anchors the top of the product mix.

**5. Revenue Trend Volatility**
Monthly revenue swung considerably throughout the year, ranging from a low of roughly R150K to a high of roughly R280K, with November as the peak month (~R274K). The absence of a clear steady upward trend suggests revenue may be more promotion- or seasonality-driven than organically growing month over month — worth investigating further with a longer historical window.

---

## Technical Approach

**Data Source:** Raw transactional sales data (Order ID, Date, Product, Product Category, Region, Sales Channel, Quantity, Revenue) — over 2,000 individual order rows.

**Build Process:**
- Structured the raw data as a clean Excel Table to support dynamic pivot table refresh as new rows are added.
- Built a set of supporting Pivot Tables (on a dedicated `Supporting_Pivots` sheet) as the calculation layer behind each chart, keeping the dashboard sheet itself free of formula clutter.
- Connected Region, Product, Product Category, and Sales Channel Slicers across all pivot tables so a single filter selection updates every chart and KPI simultaneously.
- Added Timeline/date slicers (Days, Months, Quarters, Years) to allow flexible period-over-period filtering without rebuilding any visuals.
- Used a mix of chart types matched to the analysis: line chart for revenue trend, column charts for regional/category/product comparisons, and a pie chart for sales channel share — chosen so each chart type suits the shape of the underlying question rather than defaulting to one style throughout.
