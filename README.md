# PL-300 E-Commerce Sales and Marketing Capstone

Power BI capstone project using the **Maven Fuzzy Factory e-commerce dataset** to analyze website traffic, marketing channel performance, conversion rate, product revenue, gross profit, and refund impact.

This repository is built progressively through real project commits. Each commit represents an actual completed step in the capstone work.

---

## Business Context

The business team needs an interactive Power BI report to monitor the performance of an e-commerce website across traffic sources, campaigns, devices, products, orders, revenue, profit, and refunds.

The dashboard is expected to help business users understand how website traffic turns into orders, which marketing channels generate stronger business outcomes, which products contribute the most revenue and profit, and where performance issues such as low conversion or refunds need attention.

---

## Dataset

| Item | Description |
|---|---|
| Dataset | Maven Fuzzy Factory e-commerce dataset |
| Source files | `website_sessions.csv`, `website_pageviews.csv`, `orders.csv`, `order_items.csv`, `order_item_refunds.csv`, `products.csv` |
| Source format | CSV multi-table relational dataset |
| Analysis domain | Website traffic, marketing channels, conversion, product sales, revenue, gross profit, and refunds |

---

## Initial Business Questions

1. How does traffic, order volume, and revenue change over time?
2. Which marketing channels drive the strongest business performance?
3. How does conversion rate differ by traffic source, campaign, and device type?
4. Which products generate the highest revenue and gross profit?
5. How do refunds impact product and revenue performance?
6. What recommendations can improve conversion, marketing efficiency, and product profitability?

---

## Report Pages

The current Power BI report contains:

1. Executive Summary
2. Product Analysis
3. Marketing Channel Analysis
4. Product Drillthrough
5. Tooltip Product

The report also includes slicers, drillthrough, tooltip, bookmark-based view switching, and row-level security by marketing source.

---

## Current Repository Content

| Path | Description |
|---|---|
| `README.md` | Project overview, business context, dataset summary, report scope, and documentation index |
| `data/raw/` | Raw Maven Fuzzy Factory CSV dataset files |
| `powerbi/pbix/pl300_ecommerce_capstone.pbix` | Power BI report file |
| `docs/business-scenario.md` | Detailed background and business scenario |
| `docs/data-understanding.md` | Dataset structure, table roles, and grain explanation |
| `docs/power-query-notes.md` | Import method, preparation steps, and cleanup notes |
| `docs/data-model.md` | Data model, relationships, Date table, and modeling decisions |
| `docs/dax-measures.md` | DAX measure catalog and validation checklist |
| `docs/dashboard-summary.md` | Report page summary, visuals, and interactions |
| `docs/insights-and-recommendations.md` | Preliminary insights and recommendation themes |
| `docs/limitations.md` | Dataset limitations, assumptions, and final review checklist |

---

## Project Status

The project has progressed from dataset selection and import into initial Power BI modeling, core DAX measures, draft report pages, tooltip, drillthrough, bookmark navigation, and RLS by `utm_source`.

The documentation set has been added to describe the dataset, Power Query preparation, data model, DAX measures, dashboard pages, preliminary insights, and limitations.

Before the final portfolio version, the report should still be validated and polished, especially product-level refund filtering, blank source labels, visual naming, final screenshots, and final business recommendations.