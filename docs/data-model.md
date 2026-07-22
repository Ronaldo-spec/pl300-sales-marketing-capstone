# Data Model

## Modeling Approach

The Maven Fuzzy Factory dataset is modeled as a relational e-commerce funnel. Unlike a single flat table model, this project keeps the raw business entities separated and connects them through relationships.

The model supports analysis across:

```text
website sessions -> pageviews -> orders -> order items -> refunds
                              -> products
```

This structure is more realistic than a single wide table because each table has a different grain.

---

## Main Tables

| Table | Role | Grain |
|---|---|---|
| `website_sessions` | Traffic and marketing source fact table | One row per website session |
| `website_pageviews` | Website behavior fact table | One row per pageview |
| `orders` | Order header fact table | One row per order |
| `order_items` | Sales transaction detail fact table | One row per purchased item |
| `order_item_refunds` | Refund transaction fact table | One row per refunded order item |
| `products` | Product dimension table | One row per product |
| `DimDate` | Calendar dimension | One row per date |
| `_Measures` | Measure table | Stores DAX measures only |

---

## Core Relationships

| From Table | From Column | To Table | To Column | Cardinality | Filter Direction | Purpose |
|---|---|---|---|---|---|---|
| `products` | `product_id` | `order_items` | `product_id` | 1:* | Single | Enables product-level revenue, profit, and refund analysis |
| `orders` | `order_id` | `order_items` | `order_id` | 1:* | Single | Connects order headers to item-level sales |
| `order_items` | `order_item_id` | `order_item_refunds` | `order_item_id` | 1:* or 1:1 depending on data uniqueness | Single if available | Connects refunds to the refunded item and product |
| `website_sessions` | `website_session_id` | `website_pageviews` | `website_session_id` | 1:* | Single | Connects sessions to pageviews |
| `website_sessions` | `website_session_id` | `orders` | `website_session_id` | 1:1 | Both if required by Power BI | Connects sessions to resulting orders |
| `DimDate` | `Date` | `website_sessions` | `created_date` | 1:* | Single | Provides the main active date path for funnel analysis |

---

## Date Table

A dedicated date table named `DimDate` was created to support consistent time-based analysis.

Expected columns:

| Column | Purpose |
|---|---|
| `Date` | Date key used for relationships |
| `Year` | Year grouping |
| `Month Number` | Numeric month sorting |
| `Month Name` | Month display label |
| `YearMonth` | Month-level time axis |
| `Quarter` | Quarter grouping |

The `DimDate[Date]` column should be marked as the official date column using **Mark as date table**.

---

## Date Relationship Decision

The safest model design uses `DimDate` as the main active calendar path through `website_sessions` because website sessions are the starting point of the e-commerce funnel.

Recommended active path:

```text
DimDate -> website_sessions -> orders -> order_items -> order_item_refunds
                      |
                      -> website_pageviews
```

Direct active relationships from `DimDate` to multiple downstream fact tables can create ambiguous filter paths. If additional date relationships are needed later, they should be handled carefully through inactive relationships or dedicated DAX logic.

---

## Product Relationship Decision

The `products` table should not be related to `DimDate` through `products[created_at]` or `products[created_date]` for the main sales analysis.

Reason:

- `products[created_at]` represents product creation or launch timing.
- It does not represent order, revenue, session, or refund timing.
- Product sales analysis should flow through `products[product_id]` to `order_items[product_id]`.

Correct product analysis path:

```text
products -> order_items -> orders -> website_sessions
```

---

## Model Validation Notes

Before treating the dashboard as final, validate the following:

1. Product filters must affect `order_items` measures such as revenue and gross profit.
2. Product filters must also affect refund measures through `order_items[order_item_id]` to `order_item_refunds[order_item_id]`.
3. Date filters should not produce blank or `--` year values unless the source data genuinely contains missing dates.
4. One-to-many relationships should normally use single filter direction.
5. One-to-one relationships may require both-direction filtering in Power BI.

---

## Modeling Principle

The model should preserve business meaning rather than force all tables into one combined table.

The main analytical logic is:

- sessions explain traffic and marketing source,
- orders explain conversion,
- order items explain product-level sales,
- products explain product grouping,
- refunds explain revenue reduction and product quality risk.