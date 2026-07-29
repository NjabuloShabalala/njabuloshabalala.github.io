# B2B Sales Operations, Fulfillment & Marketing Attribution Data Modeling

## Key Results

| Revenue Variance | Avg. Lead Time | Campaign ROAS | Active Accounts |
|:---:|:---:|:---:|:---:|
| **-5.0%** | **8.71 days** | **1.71x** | **47** |

---

## Executive Summary: Commercial Operations & Campaign Performance

### 1. Revenue Target Deficit (-5% Variance)
Performance Gap: Total actual sales revenue reached R526.64K against a target quota of R554.36K, representing an overall -5.0% revenue shortfall.

Seasonality Trends: The line chart reveals that revenue tracking closely matched targets during Q1 and late Q4, but experienced a significant drop during mid-year periods (specifically Months 5, 7, and 11), driving the overall annual deficit.

Regional Contribution: Middle East (R130.01K) and Asia Pacific (R105.65K) generated the highest regional revenue, while Latin America (R84.48K) lagged behind with the lowest sales volume (12 orders) and lowest average discount applied (2.81%).

### 2. Operational Fulfillment Baseline (8.71 Days Lead Time)
Fulfillment Efficiency: Across all 80 processed orders, the business maintains an average order-to-delivery lead time of 8.71 days.

Account Manager Variability: Delivery performance varies across account manager portfolios:

David Park's client orders achieve the fastest dispatch-to-delivery cycle at 7.9 days.

Omar Khan's client orders experience the longest fulfillment timelines, averaging 9.2 days.

Process Bottlenecks: Order-to-delivery cycle times remain flat across geographic territories, indicating that fulfillment bottlenecks stem from warehouse processing and logistics handoffs rather than regional transit distances.

### 3. Marketing Campaign Efficiency & Attribution (1.71x ROAS)
Ad Spend vs. Revenue Generation: Total campaign spend of R78.84K generated R135.21K in directly attributed sales revenue across covered product categories, yielding a 1.71x Return on Ad Spend (ROAS).

Top Campaign Drivers:

Black Friday represented the single largest ad spend commitment (~R30K), driving strong conversion across high-margin product lines.

Summer Sale and Spring Launch 2026 demonstrated high capital efficiency, generating substantial attributed sales relative to lower budget allocations.

### 4. Account Concentration & Payment Risk Profile
Revenue Concentration: Out of 47 active corporate accounts, lifetime value is heavily concentrated in the top 10% of clients. Summit Commerce (R32.51K LTV) and Cascade Retail (R29.20K LTV) represent the primary drivers of enterprise volume.

Terms Exposure: The majority of high-volume clients operate under Net 30 and Net 60 terms. Establishing formal credit monitoring for top-tier accounts (e.g., Vanguard Holdings, Zenith Group) is recommended to mitigate working capital risk on deferred invoice cycles.

---

## The Problem

A growing B2B enterprise was struggling with inconsistent commercial reporting, unsegmented campaign tracking, and poor dashboard performance. Because transactional data—spanning across sales orders, order-fulfillment lifecycle events, target quotas, and promotional campaign spend—was captured across disconnected tables and dumped into an unmodeled schema, leadership lacked a unified source of truth. Different departments reported conflicting revenue targets, sales variances, and campaign performance. Furthermore, operations could not evaluate delivery lead times across product lines or regional territories without manually stitching together raw data files.

## Operational & Data Architecture Friction

Before I restructured the data model, the reporting environment suffered from three core structural flaws:

Siloed Process Tracking: Sales transactions (facts_sales) and order fulfillment events (facts_order_process) were maintained as separate fact tables without an integrated dimension model, preventing cross-functional operational analysis.

Complex Many-to-Many Relationships: Marketing campaigns were disconnected from actual sales transactions. Without a bridge table approach, attributing revenue to specific promotional spend resulted in blank visuals or revenue duplication.

Grain & Schema Misalignment: Comparing monthly target quotas (fact_sales_targets) against daily transactional sales (facts_sales) caused filter context issues and broken visual trends across standard time dimensions.

---

## Dashboard Showcase

### Page 1: Sales Performance & Fulfillment Operations
![Page 1 Overview](assets/visual-1-power-bi.png)
*Figure 1: Executive view tracking revenue variance against targets, fulfillment lead times, and regional sales distribution.*

### Page 2: Customer Portfolio & Campaign Performance
![Page 2 Overview](assets/visual-2-power-bi.png)
*Figure 2: Corporate account LTV distribution alongside campaign ad spend vs. attributed revenue.*

---

## Business Objectives & Core Questions

To establish operational visibility, the raw schema was restructured into a performant Star Schema with explicit DAX relationship-bridging patterns, answering the four key business questions that stakeholders and leadership had:

**1. Sales Target Variance**
The Question was: Is the business hitting its monthly revenue targets, and what is the exact percentage variance between actual sales revenue and target quotas?

Metrics used: `[actual_revenue]`, `[target_revenue]`, `[revenue_variance_%]`

**2. Operational Fulfillment Lead Times**
What is the average order-to-delivery cycle time across regions, account managers, and customer payment terms?

Metrics used: `[order_to_delivery_days]`

**3. Customer Portfolio & LTV Concentration**
How is revenue concentrated across corporate accounts, and what is the lifetime value (LTV) profile of clients managed under varying payment terms?

Metrics used: `[lifetime_value]`, `[total_orders]`, `[total_active_customers]`

**4. Commercial Campaign Attribution**
What is the true Return on Ad Spend (ROAS) per campaign when filtering sales revenue strictly by the products covered in each marketing campaign?

Metrics used: `[attributed_campaign_revenue]`, `[total_spend]`, `[ROAS]`

---

## Key Takeaways & Technical Architecture

This project transforms raw transactional data into an enterprise Star Schema in Power BI:

Filter Context Resolution: Applied TREATAS in DAX to pass promotional product filter contexts through fact_promotion_coverage directly to dim_product, resolving non-propagating filters between marketing spend and actual sales.

Shared Conformed Dimensions: Modeled dim_date, dim_customer, and dim_geography to seamlessly filter disparate fact tables (facts_sales, facts_order_process, fact_sales_targets, and fact_campaign_spend).

Interactive 2-Page Executive Dashboard: Designed a structured, 2-page interactive Power BI reporting suite covering Sales Performance & Fulfillment Operations on Page 1 and Customer Portfolio & Campaign Performance on Page 2.

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
```

</details>
    ALLEXCEPT(dim_customer, dim_customer[customer_id], dim_customer[customer_company_name])
)
