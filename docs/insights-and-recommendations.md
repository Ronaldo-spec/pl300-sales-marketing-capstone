# Insights and Recommendations

## Status

This document records preliminary insights from the current draft dashboard.

These findings should be reviewed again after final validation of relationships, refund calculations, formatting, and report layout.

---

## Current High-Level Metrics

Based on the current draft report, the dashboard tracks the following headline metrics:

| Metric | Current Draft Value |
|---|---:|
| Total Sessions | 472,871 |
| Total Orders | 32K |
| Conversion Rate | 6.83% |
| Total Revenue | $193,850,975 |
| Gross Profit | $121,613,950 |
| Net Revenue | $185,317,106 |
| Refund Rate | 4.32% |
| Refund Count | 1,731 |

These figures provide the executive baseline for evaluating traffic, conversion, revenue, profit, and refund performance.

---

## Preliminary Insight 1: Traffic converts into meaningful order volume

The current draft shows 472,871 sessions and around 32K orders, resulting in a conversion rate of 6.83%.

### Interpretation

The business has a measurable conversion funnel from website traffic into orders. Conversion rate should be monitored by traffic source, campaign, and device type because traffic volume alone does not guarantee business value.

### Recommendation

Prioritize marketing performance review around conversion, not only sessions. A traffic source with fewer sessions but stronger conversion may be more valuable than a high-traffic but low-conversion channel.

---

## Preliminary Insight 2: Revenue and profit are concentrated in key products

The Product Analysis page indicates that **The Original Mr. Fuzzy** contributes the highest revenue and gross profit in the current draft report.

### Interpretation

A small number of products may be driving a large share of business value. This creates an opportunity to protect and optimize high-performing products, but it may also create dependency risk.

### Recommendation

Use product-level analysis to monitor revenue concentration, gross profit, and refund rate together. A product should not be evaluated by revenue alone.

---

## Preliminary Insight 3: Marketing channels show different conversion behavior

The Marketing Channel Analysis page compares sessions, orders, and conversion rate by `utm_source`.

The current draft shows visible differences between sources such as `gsearch`, `bsearch`, `socialbook`, and blank or direct traffic.

### Interpretation

Traffic sources do not perform equally. Some channels may generate more traffic and orders, while others may have weaker conversion.

### Recommendation

Compare each channel using at least four metrics together:

1. Sessions
2. Orders
3. Conversion Rate
4. Revenue

This avoids overvaluing a channel based only on traffic volume.

---

## Preliminary Insight 4: Refund performance needs product-level validation

The draft Product Analysis page includes refund amount and refund rate by product.

### Interpretation

Refund rate is important because it can reduce net revenue and may indicate product quality, expectation mismatch, or fulfillment issues.

However, refund analysis must be validated carefully because refunds are stored in a separate table and need a correct relationship path through `order_items`.

### Recommendation

Before publishing final insights, validate that product filters affect:

- `Total Refund Amount`,
- `Refund Count`,
- `Refund Rate`,
- `Net Revenue`.

The expected relationship path is:

```text
products -> order_items -> order_item_refunds
```

If refund values do not change correctly by product, fix the relationship or use a relationship-safe DAX pattern.

---

## Preliminary Insight 5: Product drillthrough improves diagnostic analysis

The Product Drillthrough page allows a selected product to be reviewed in detail.

### Interpretation

This helps users move from high-level product ranking into a specific product's revenue trend, profit, orders, and refund impact.

### Recommendation

Use the drillthrough page as the main diagnostic path for product-level review. Keep the Executive Summary high-level and use drillthrough for deeper analysis.

---

## Final Recommendation Themes

The final recommendation section should be organized around these business actions:

| Theme | Recommendation Direction |
|---|---|
| Marketing efficiency | Shift attention from traffic volume alone to sessions, conversion, orders, and revenue together |
| Product performance | Prioritize products with high revenue and strong gross profit |
| Refund control | Investigate products with high refund rate or high refund amount |
| Funnel monitoring | Track sessions, orders, and conversion rate over time |
| Dashboard adoption | Use drillthrough and tooltip features to support executive-to-detail analysis |

---

## Final Validation Required

Before the final portfolio version, validate:

1. Product-level refund numbers.
2. Blank or `NULL` source/category values.
3. Year slicer blank values.
4. RLS behavior for each `utm_source` role.
5. Bookmark behavior for revenue and conversion views.
6. Tooltip readability.
7. Final dashboard screenshots for README presentation.