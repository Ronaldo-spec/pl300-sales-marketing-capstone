# DAX Measure Catalog

## Measure Table

All reusable business calculations are stored in the `_Measures` table.

This keeps the report cleaner because analytical logic is separated from the raw data tables.

---

## Traffic Measures

### Total Sessions

```DAX
Total Sessions =
DISTINCTCOUNT(website_sessions[website_session_id])
```

Counts unique website sessions.

### Total Pageviews

```DAX
Total Pageviews =
DISTINCTCOUNT(website_pageviews[website_pageview_id])
```

Counts unique pageviews.

### Pages per Session

```DAX
Pages per Session =
DIVIDE([Total Pageviews], [Total Sessions])
```

Measures average pageviews per website session.

---

## Order and Conversion Measures

### Total Orders

```DAX
Total Orders =
DISTINCTCOUNT(orders[order_id])
```

Counts unique orders.

### Conversion Rate

```DAX
Conversion Rate =
DIVIDE([Total Orders], [Total Sessions])
```

Measures the percentage of sessions that convert into orders.

### Average Order Value

```DAX
Average Order Value =
DIVIDE([Total Revenue], [Total Orders])
```

Measures average revenue per order.

---

## Revenue and Profit Measures

### Total Revenue

```DAX
Total Revenue =
SUM(order_items[price_usd])
```

Calculates gross item-level revenue.

### Total COGS

```DAX
Total COGS =
SUM(order_items[cogs_usd])
```

Calculates item-level cost of goods sold.

### Gross Profit

```DAX
Gross Profit =
[Total Revenue] - [Total COGS]
```

Calculates gross profit before refund deduction.

### Net Revenue

```DAX
Net Revenue =
[Total Revenue] - [Total Refund Amount]
```

Calculates revenue after refund deduction.

---

## Refund Measures

### Total Refund Amount

```DAX
Total Refund Amount =
SUM(order_item_refunds[refund_amount_usd])
```

Calculates the total refunded amount.

If product-level refund amount does not respond correctly to product filters, validate the relationship between `order_items[order_item_id]` and `order_item_refunds[order_item_id]`.

A relationship-safe alternative is:

```DAX
Total Refund Amount =
CALCULATE(
    SUM(order_item_refunds[refund_amount_usd]),
    TREATAS(
        VALUES(order_items[order_item_id]),
        order_item_refunds[order_item_id]
    )
)
```

### Refund Count

```DAX
Refund Count =
DISTINCTCOUNT(order_item_refunds[order_item_refund_id])
```

Counts refunded order items.

If product-level refund count does not respond correctly to product filters, use the same relationship validation logic as `Total Refund Amount`.

### Order Item Count

```DAX
Order Item Count =
DISTINCTCOUNT(order_items[order_item_id])
```

Counts item-level transactions.

### Refund Rate

```DAX
Refund Rate =
DIVIDE([Refund Count], [Order Item Count])
```

Measures refund frequency relative to total order items.

---

## Selection Measure

### Selected Product Name

```DAX
Selected Product Name =
SELECTEDVALUE(products[product_name], "All Products")
```

Displays the selected product name on the drillthrough page.

---

## Recommended Formatting

| Measure | Format |
|---|---|
| `Total Sessions` | Whole number |
| `Total Pageviews` | Whole number |
| `Total Orders` | Whole number |
| `Order Item Count` | Whole number |
| `Refund Count` | Whole number |
| `Total Revenue` | Currency |
| `Total COGS` | Currency |
| `Gross Profit` | Currency |
| `Net Revenue` | Currency |
| `Total Refund Amount` | Currency |
| `Average Order Value` | Currency |
| `Conversion Rate` | Percentage |
| `Refund Rate` | Percentage |
| `Pages per Session` | Decimal number |

---

## Validation Checklist

Before publishing the final report, validate these points:

1. `Total Revenue` changes when filtering by product.
2. `Gross Profit` changes when filtering by product.
3. `Total Refund Amount` changes when filtering by product.
4. `Refund Rate` changes logically when filtering by product.
5. `Conversion Rate` changes when filtering by `utm_source`, `utm_campaign`, and `device_type`.
6. `Total Orders` should not exceed `Total Sessions` for the same filter context.
7. `Net Revenue` should equal `Total Revenue - Total Refund Amount`.

These checks are important because the dataset uses several related fact tables rather than one flat table.