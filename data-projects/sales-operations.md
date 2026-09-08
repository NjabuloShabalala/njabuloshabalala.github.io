<style>
body {
    background-color: #0b0f17 !important;
    color: #f3f4f6 !important;
}
h1, h2, h3 {
    color: #ffffff !important;
}
a {
    color: #3b82f6 !important;
}
table, .markdown-body table {
    border-collapse: collapse !important;
    width: 100% !important;
    background-color: #0b0f17 !important;
}
table th, table td, .markdown-body table th, .markdown-body table td {
    border: 1px solid #212d3d !important;
    padding: 0.6rem 1rem !important;
    background-color: #131b26 !important;
    color: #f3f4f6 !important;
}
table th, .markdown-body table th {
    background-color: #131b26 !important;
    color: #38bdf8 !important;
}
table tr:nth-child(even), .markdown-body table tr:nth-child(even) {
    background-color: #131b26 !important;
}
code {
    background-color: #131b26 !important;
    color: #38bdf8 !important;
}
pre {
    background-color: #131b26 !important;
    border: 1px solid #212d3d !important;
}
details {
    background-color: #131b26 !important;
    border: 1px solid #212d3d !important;
}
summary {
    color: #38bdf8 !important;
}
img {
    border-radius: 8px;
    border: 1px solid #212d3d;
}
</style>

# B2B Sales Operations, Fulfillment and Marketing Attribution Data Modeling

## Key Results

| Revenue Variance | Avg. Lead Time | Campaign ROAS | Active Accounts |
|:---:|:---:|:---:|:---:|
| **-5.0%** | **8.71 days** | **1.71x** | **47** |

---

## The Business Problem

A growing B2B enterprise was struggling with inconsistent commercial reporting, unsegmented campaign tracking, and poor dashboard performance. Transactional data, spanning sales orders, order-fulfillment lifecycle events, target quotas, and promotional campaign spend, was captured across disconnected tables in an unmodeled schema, so leadership lacked a single source of truth. Different departments reported conflicting revenue targets, sales variances, and campaign performance. Operations couldn't evaluate delivery lead times across product lines or regional territories without manually stitching together raw data files. The core question this project answers: is the business actually hitting its targets, where is fulfillment breaking down, and is marketing spend paying for itself, once the data is modeled to answer all three at once?

## Plausible Causes of the Reporting Breakdown

Before restructuring anything, it was worth naming what typically produces this kind of fragmented reporting, since the fix depends on the cause:

- **Siloed process tracking:** sales transactions (`facts_sales`) and order fulfillment events (`facts_order_process`) were maintained as separate fact tables without an integrated dimension model, which would prevent any cross-functional operational analysis.
- **Complex many-to-many relationships left unbridged:** marketing campaigns were disconnected from actual sales transactions. Without a bridge table approach, attributing revenue to specific promotional spend would produce either blank visuals or duplicated revenue.
- **Grain and schema misalignment:** comparing monthly target quotas (`fact_sales_targets`) against daily transactional sales (`facts_sales`) would cause filter context issues and broken visual trends across standard time dimensions.

Each of these was tested directly by restructuring the schema and checking whether the resulting model could answer the four business questions below without the errors these causes would predict.

## What I Built

The raw schema was restructured into a performant Star Schema with explicit DAX relationship-bridging patterns, built to answer four business questions:

**1. Sales Target Variance**
Is the business hitting its monthly revenue targets, and what is the exact percentage variance between actual sales revenue and target quotas?

Metrics used: `[actual_revenue]`, `[target_revenue]`, `[revenue_variance_%]`

**2. Operational Fulfillment Lead Times**
What is the average order-to-delivery cycle time across regions, account managers, and customer payment terms?

Metrics used: `[order_to_delivery_days]`

**3. Customer Portfolio and LTV Concentration**
How is revenue concentrated across corporate accounts, and what is the lifetime value (LTV) profile of clients managed under varying payment terms?

Metrics used: `[lifetime_value]`, `[total_orders]`, `[total_active_customers]`

**4. Commercial Campaign Attribution**
What is the true Return on Ad Spend (ROAS) per campaign when filtering sales revenue strictly by the products covered in each marketing campaign?

Metrics used: `[attributed_campaign_revenue]`, `[total_spend]`, `[ROAS]`

---

## Dashboard Showcase

### Page 1: Sales Performance and Fulfillment Operations
![Page 1 Overview](/assets/visual-1-power-bi.png)
*Figure 1: Executive view tracking revenue variance against targets, fulfillment lead times, and regional sales distribution.*

### Page 2: Customer Portfolio and Campaign Performance
![Page 2 Overview](/assets/visual-2-power-bi.png)
*Figure 2: Corporate account LTV distribution alongside campaign ad spend vs. attributed revenue.*

📁 **[Download Power BI Template (.pbit)](/assets/portfolio-project.pbit)**

---

## Proof: What Each Question's Answer Shows

### 1. Revenue Target Deficit (-5% Variance)
Total actual sales revenue reached R526.64K against a target quota of R554.36K, a -5.0% shortfall overall. The line chart shows revenue tracking closely with targets during Q1 and late Q4, then dropping significantly during mid-year periods, specifically Months 5, 7, and 11, which drove the annual deficit. Regionally, Middle East (R130.01K) and Asia Pacific (R105.65K) generated the most revenue, while Latin America (R84.48K) lagged with the lowest sales volume (12 orders) and the lowest average discount applied (2.81%).

### 2. Operational Fulfillment Baseline (8.71 Days Lead Time)
Across all 80 processed orders, the business maintains an average order-to-delivery lead time of 8.71 days. Delivery performance varies by account manager: David Park's client orders achieve the fastest dispatch-to-delivery cycle at 7.9 days, while Omar Khan's client orders run longest at 9.2 days. Order-to-delivery cycle times stay flat across geographic territories, which points the bottleneck toward warehouse processing and logistics handoffs rather than regional transit distance, directly confirming the siloed-process-tracking cause named above: once fulfillment and sales data were joined, the bottleneck became visible by manager rather than by geography.

### 3. Marketing Campaign Efficiency and Attribution (1.71x ROAS)
Total campaign spend of R78.84K generated R135.21K in directly attributed sales revenue across covered product categories, a 1.71x Return on Ad Spend. Black Friday represented the single largest ad spend commitment (roughly R30K), driving strong conversion across high-margin product lines. Summer Sale and Spring Launch 2026 showed high capital efficiency, generating substantial attributed sales relative to their lower budget allocations. This result depended entirely on solving the many-to-many bridging problem named above: without the `TREATAS` pattern connecting campaigns to products to actual sales, this number simply wasn't computable before.

### 4. Account Concentration and Payment Risk Profile
Out of 47 active corporate accounts, lifetime value is heavily concentrated in the top 10% of clients. Summit Commerce (R32.51K LTV) and Cascade Retail (R29.20K LTV) are the primary drivers of enterprise volume. Most high-volume clients operate under Net 30 and Net 60 terms. Establishing formal credit monitoring for top-tier accounts (such as Vanguard Holdings and Zenith Group) is recommended to mitigate working capital risk on deferred invoice cycles.

---

## Recommendations

Investigate the Months 5, 7, and 11 revenue dips specifically, since they're what's driving the overall -5% shortfall rather than a broad, even underperformance. Given fulfillment delay tracks by account manager and not by region, the fix belongs in process review with the manager whose orders run slowest, not in regional logistics. Black Friday's outsized spend paid off, but Summer Sale and Spring Launch 2026 delivered better efficiency per rand spent, worth a closer look at reallocating budget toward the more capital-efficient campaigns. Given how concentrated LTV is in a small number of top accounts, formal credit monitoring for Vanguard Holdings and Zenith Group specifically should be treated as a near-term priority rather than a general policy update.

---

## Key Takeaways and Technical Architecture

This project transforms raw transactional data into an enterprise Star Schema in Power BI:

**Filter Context Resolution:** Applied `TREATAS` in DAX to pass promotional product filter contexts through `fact_promotion_coverage` directly to `dim_product`, resolving non-propagating filters between marketing spend and actual sales.

**Shared Conformed Dimensions:** Modeled `dim_date`, `dim_customer`, and `dim_geography` to seamlessly filter disparate fact tables (`facts_sales`, `facts_order_process`, `fact_sales_targets`, and `fact_campaign_spend`).

**Interactive 2-Page Executive Dashboard:** Designed a structured, 2-page interactive Power BI reporting suite covering Sales Performance and Fulfillment Operations on Page 1 and Customer Portfolio and Campaign Performance on Page 2.

<details>
<summary><b>Click to expand full DAX Measure Codebook</b></summary>

```dax
/* =================================================================
   1. REVENUE & TARGET VARIANCE MEASURES
   ================================================================= */

// Total Actual Revenue from Sales Fact
actual_revenue = 
SUM(facts_sales[line_total])

// Total Target Revenue from Sales Targets Fact
target_revenue = 
SUM(fact_sales_targets[target_revenue])

// Revenue Variance Percentage against Target Quota
revenue_variance_% = 
VAR VarianceAmount = [actual_revenue] - [target_revenue]
RETURN
DIVIDE(VarianceAmount, [target_revenue], 0)


/* =================================================================
   2. FULFILLMENT & OPERATIONAL MEASURES
   ================================================================= */

// Average Order-to-Delivery Lead Time in Days
order_to_delivery_days = 
AVERAGEX(
    facts_order_process,
    DATEDIFF(facts_order_process[order_date], facts_order_process[delivery_date], DAY)
)

// Average Order-to-Invoice Lead Time in Days
order_to_invoice_days = 
AVERAGEX(
    facts_order_process,
    DATEDIFF(facts_order_process[order_date], facts_order_process[invoice_date], DAY)
)

// Average Discount Percentage Across Orders
avg_discount = 
AVERAGE(facts_sales[discount])


/* =================================================================
   3. MARKETING ATTRIBUTION & CAMPAIGN MEASURES
   ================================================================= */

// Total Ad Spend Across Campaigns
total_spend = 
SUM(fact_campaign_spend[spend])

// Revenue Attributed to Campaigns via Product Coverage Bridge Table
attributed_campaign_revenue = 
VAR CurrentProducts = 
    CALCULATETABLE(
        VALUES(fact_promotion_coverage[product_key])
    )
RETURN
CALCULATE(
    [actual_revenue],
    REMOVEFILTERS(dim_campaign),
    TREATAS(CurrentProducts, dim_product[product_key])
)

// Return on Ad Spend (ROAS) Ratio
ROAS = 
DIVIDE([attributed_campaign_revenue], [total_spend], 0)


/* =================================================================
   4. CUSTOMER PORTFOLIO & LTV MEASURES
   ================================================================= */

// Total Active B2B Customer Accounts
total_active_customers = 
DISTINCTCOUNT(facts_sales[customer_id])

// Customer Lifetime Value (Total Sales per Account)
lifetime_value = 
CALCULATE(
    [actual_revenue],
    ALLEXCEPT(dim_customer, dim_customer[customer_id], dim_customer[customer_company_name])
)
```

</details>
