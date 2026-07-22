# Data Understanding

## Dataset Overview

This project uses the Maven Fuzzy Factory e-commerce dataset. The dataset is stored as multiple CSV files and represents an e-commerce funnel from website traffic to product sales and refunds.

The analysis is based on six raw data tables:

| Source File | Table in Power BI | Analytical Role |
|---|---|---|
| `website_sessions.csv` | `website_sessions` | Website session and marketing traffic fact table |
| `website_pageviews.csv` | `website_pageviews` | Pageview activity fact table |
| `orders.csv` | `orders` | Order header transaction table |
| `order_items.csv` | `order_items` | Order item transaction detail table |
| `order_item_refunds.csv` | `order_item_refunds` | Refund transaction table |
| `products.csv` | `products` | Product dimension table |

The data dictionary file is used as a reference document and is not required as a model table in Power BI.

---

## Table Roles and Grain

### `website_sessions`

**Grain:** one row per website session.

This table represents the beginning of the e-commerce funnel. It is used to analyze traffic volume, marketing source, campaign performance, device behavior, and conversion into orders.

Key fields:

| Column | Description |
|---|---|
| `website_session_id` | Unique identifier for a website session |
| `created_at` | Original session timestamp |
| `created_date` | Date-only version of `created_at`, used for date analysis |
| `user_id` | User identifier |
| `is_repeat_session` | Indicates whether the session is repeat traffic |
| `utm_source` | Marketing traffic source |
| `utm_campaign` | Marketing campaign |
| `utm_content` | Campaign content detail |
| `device_type` | Device used in the session |
| `http_referer` | Referring source or URL |

### `website_pageviews`

**Grain:** one row per pageview.

This table represents website behavior inside sessions. It is useful for pageview volume and page-level analysis.

Key fields:

| Column | Description |
|---|---|
| `website_pageview_id` | Unique pageview identifier |
| `website_session_id` | Session identifier that connects pageviews to sessions |
| `created_at` | Original pageview timestamp |
| `created_date` | Date-only version of `created_at` |
| `pageview_url` | URL or page path viewed by the user |

### `orders`

**Grain:** one row per order.

This table represents order-level transactions. It is used to count orders and connect order activity back to sessions.

Key fields:

| Column | Description |
|---|---|
| `order_id` | Unique order identifier |
| `website_session_id` | Session that produced the order |
| `user_id` | User identifier |
| `created_at` | Original order timestamp |
| `created_date` | Date-only version of `created_at` |
| `primary_product_id` | Primary product in the order |
| `items_purchased` | Number of items purchased |
| `price_usd` | Order price amount |
| `cogs_usd` | Cost of goods sold at order level |

### `order_items`

**Grain:** one row per item purchased within an order.

This table is the main sales detail table. It connects orders to products and supports product-level revenue, cost, and gross profit analysis.

Key fields:

| Column | Description |
|---|---|
| `order_item_id` | Unique order item identifier |
| `order_id` | Order identifier |
| `product_id` | Product identifier |
| `created_at` | Original order item timestamp |
| `created_date` | Date-only version of `created_at` |
| `is_primary_item` | Indicates whether the item is the primary item in the order |
| `price_usd` | Item revenue amount |
| `cogs_usd` | Item cost amount |

### `order_item_refunds`

**Grain:** one row per refunded order item.

This table is used to analyze refund count, refund amount, refund rate, and net revenue impact.

Key fields:

| Column | Description |
|---|---|
| `order_item_refund_id` | Unique refund identifier |
| `order_item_id` | Refunded order item identifier |
| `order_id` | Related order identifier |
| `created_at` | Original refund timestamp |
| `created_date` | Date-only version of `created_at` |
| `refund_amount_usd` | Refund amount |

### `products`

**Grain:** one row per product.

This table is the product dimension and is used to group revenue, gross profit, refund rate, and net revenue by product name.

Key fields:

| Column | Description |
|---|---|
| `product_id` | Unique product identifier |
| `created_at` | Product creation timestamp |
| `product_name` | Product name |

---

## Analytical Focus

The dataset supports the following analysis areas:

1. Website traffic trends
2. Marketing channel performance
3. Conversion from sessions to orders
4. Product revenue and gross profit contribution
5. Refund impact by product
6. Net revenue after refunds

The dataset does not provide country or region fields, so the original country analysis requirement is replaced with marketing channel analysis based on `utm_source`, `utm_campaign`, and `device_type`.