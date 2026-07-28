# B2B Order-to-Cash Data Modeling & Schema Optimization # 
---
# 1. The Problem #
Here we have a growing B2B enterprise that was struggling with unreliable reporting, skewed financial metrics, and severe dashboard performance degradation.
Because the transactional data, which spanned sales orders, shipments, invoicing, collections, and marketing campaigns, was captured across disparate systems and dumped into an unmodeled data schema, different departments reported conflicting numbers for revenue, fulfillment lead times, and account balances. 
The Leadership team  lacked a single source of truth to evaluate the company's performance across the end-to-end Order-to-Cash (O2C) lifecycle.

# 2. Operational & Data Friction (The "Nightmare" Baseline) #
Before the modeling, the reporting environment suffered from four major structural flaws:
* Data Fan-Out & Grain Mismatch: Merging multi-contact customer details directly into transactional tables created many-to-many relationships, inflating revenue totals and producing false aggregation numbers.
* Siloed Process Tracking: Order placement, physical fulfillment (shipping), financial invoicing, and payment collections existed as disconnected event tables. Operations couldn't track order progress sequentially through the pipeline.
* Low Stakeholder Trust: Discrepancies between billed revenue (Invoices) and realized cash (Payments) eroded leadership’s confidence in BI dashboards.
* Resource Inefficiency: Unpruned source tables with hundreds of redundant columns severely bloated file sizes, lengthened refresh cycles, and slowed down visual queries.

# 3. Business Objectives & the questions to answer #
To establish operational clarity, the data model needed to be restructured into an enterprise-grade Star Schema which would be capable of answering the four core operational questions:
1. Financial Reconciliation (Invoiced vs. Realized Cash): What is the exact variance between billed revenue and actual cash collected, and which B2B accounts drive outstanding balances or payment defaults?
2. Order Fulfillment Efficiency: What is the average cycle time from order placement to shipment dispatch, and where are the fulfillment bottlenecks across product lines?
3. Account Health & Lifecycle: How do individual corporate accounts perform when evaluated by total order volume, average payment delay, and lifetime value—without distorting metrics across multiple contact points?
4. Commercial Attributability: Which marketing campaigns drive actionable pipeline orders and downstream bottom-line revenue versus unfulfilled demand?
--- 
# 4. Key Takeaways #
This project transforms an unoptimized, multi-grain transactional dataset into a clean, performant Star Schema. By isolating entity dimensions from event facts and standardizing relationships, the model resolves revenue inflation risks, optimizes query speed, and enables unified Order-to-Cash tracking across Finance, Logistics, and Sales.
