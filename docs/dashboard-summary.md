# Dashboard Summary

## Report Structure

The Power BI report contains five pages:

1. Executive Summary
2. Product Analysis
3. Marketing Channel Analysis
4. Product Drillthrough
5. Tooltip Product

The report follows the Maven Fuzzy Factory e-commerce funnel and focuses on traffic, conversion, revenue, product performance, and refunds.

---

## Page 1: Executive Summary

### Purpose

The Executive Summary page provides a high-level view of overall e-commerce performance.

It is designed for business users who need to quickly understand whether traffic, orders, conversion, revenue, and refund performance are healthy.

### Main Visuals

| Visual | Purpose |
|---|---|
| KPI cards | Show high-level metrics such as sessions, orders, conversion rate, revenue, gross profit, and refund rate |
| Year trend chart | Shows order or revenue movement over time |
| Product revenue chart | Shows product contribution to total revenue |
| Conversion by traffic source chart | Shows conversion performance by `utm_source` |
| Year slicer | Allows year-level filtering |
| Device type slicer | Allows filtering by device behavior |
| Bookmark buttons | Switch between revenue-focused and conversion-focused views |

### Business Questions Answered

- How does traffic, order volume, and revenue change over time?
- Which business metrics should be monitored at executive level?
- Which products or traffic sources contribute most to current performance?

---

## Page 2: Product Analysis

### Purpose

The Product Analysis page evaluates product-level contribution to revenue, gross profit, refund rate, and net performance.

### Main Visuals

| Visual | Purpose |
|---|---|
| Revenue by product | Identifies highest revenue products |
| Gross profit by product | Identifies highest profit contributors |
| Refund rate by product | Highlights products with higher refund risk |
| Product performance matrix | Compares revenue, COGS, gross profit, refund amount, and refund rate |
| Product slicer | Allows product-level focus |
| Year slicer | Allows time-based filtering |

### Business Questions Answered

- Which products generate the highest revenue?
- Which products generate the highest gross profit?
- Which products have the highest refund rate?
- Which products should be prioritized, monitored, or investigated further?

### Validation Note

Product-level refund metrics must be validated carefully because refund data is stored in a separate table. Product filters should affect refund metrics through the relationship path:

```text
products -> order_items -> order_item_refunds
```

---

## Page 3: Marketing Channel Analysis

### Purpose

The Marketing Channel Analysis page replaces the original country analysis requirement because the final dataset does not contain country or region fields.

This page analyzes performance across traffic source, campaign, and device type.

### Main Visuals

| Visual | Purpose |
|---|---|
| Total sessions by `utm_source` | Identifies channels that drive traffic |
| Total orders by `utm_source` | Identifies channels that generate orders |
| Conversion rate by `utm_source` | Compares channel efficiency |
| Campaign slicer/cards | Supports campaign-level exploration |
| Device type slicer/cards | Supports device-level exploration |

### Business Questions Answered

- Which marketing channels generate the most sessions?
- Which channels generate the most orders?
- Which sources, campaigns, or devices convert better?
- Which channels may need optimization?

---

## Page 4: Product Drillthrough

### Purpose

The Product Drillthrough page provides detailed analysis for a selected product.

Users can drill through from product-level visuals and inspect the selected product's revenue, profit, orders, refund rate, and channel contribution.

### Drillthrough Field

```text
products[product_name]
```

### Main Visuals

| Visual | Purpose |
|---|---|
| Selected product label | Displays the product being analyzed |
| KPI cards | Show product-specific revenue, gross profit, net revenue, orders, and refund rate |
| Revenue trend by year | Shows selected product revenue over time |
| Orders by traffic source | Shows which source drives orders for the selected product |
| Summary matrix | Shows revenue, COGS, gross profit, refund count, and refund amount |
| Back button | Returns the user to the previous report page |

### Business Questions Answered

- How does one selected product perform in detail?
- What is the selected product's revenue trend?
- Which traffic sources contribute to the selected product's orders?
- How much refund impact does the selected product have?

---

## Page 5: Tooltip Product

### Purpose

The Tooltip Product page provides a compact contextual view when users hover over product-level visuals.

### Main Visuals

| Visual | Purpose |
|---|---|
| Total Revenue card | Shows product-specific revenue in the hover context |
| Gross Profit card | Shows product-specific profit in the hover context |
| Refund Rate card | Shows product-specific refund rate in the hover context |
| Mini chart | Shows a compact breakdown such as revenue by traffic source |

### Design Note

Tooltip pages should be compact and easy to read. If the mini chart becomes unreadable, it is better to keep only high-value KPI cards.

---

## Interactivity

| Feature | Implementation |
|---|---|
| Slicers | Year, product, traffic source, campaign, and device type filters |
| Drillthrough | Product-level drillthrough using `products[product_name]` |
| Tooltip | Product tooltip page for hover-level details |
| Bookmark/Button | Revenue view and conversion view navigation on Executive Summary |
| RLS | Marketing-channel-based roles using `website_sessions[utm_source]` |

---

## Current Review Notes

The current dashboard already covers the required capstone interactions. Before final presentation, the report should be polished by:

1. validating product-level refund calculations,
2. cleaning `NULL` or `--` display values where appropriate,
3. reducing heavy borders if the final dashboard is intended for portfolio display,
4. renaming technical titles such as `product_name` and `utm_source` into business-friendly labels,
5. adding final screenshots to the repository after the layout is cleaned.