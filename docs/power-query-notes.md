# Power Query Preparation Notes

## Import Method

The Maven Fuzzy Factory dataset is provided as separate CSV files. Each CSV file was imported into Power BI using:

```text
Home > Get data > Text/CSV
```

The files were imported one by one because each file represents a different logical table. The `Folder` connector and `Combine` action were not used because the files do not share the same schema.

---

## Imported Tables

| CSV File | Power BI Query |
|---|---|
| `products.csv` | `products` |
| `orders.csv` | `orders` |
| `order_items.csv` | `order_items` |
| `order_item_refunds.csv` | `order_item_refunds` |
| `website_sessions.csv` | `website_sessions` |
| `website_pageviews.csv` | `website_pageviews` |

The data dictionary file is kept as documentation and does not need to be loaded as a report table.

---

## Core Preparation Steps

The following preparation steps were applied or validated in Power Query:

1. **Use first row as headers**
   - CSV headers were promoted so field names such as `product_id`, `order_id`, `website_session_id`, and `created_at` are recognized as column names.

2. **Data type validation**
   - ID fields were checked as whole number fields.
   - Timestamp fields such as `created_at` were checked as date/time fields.
   - Monetary fields such as `price_usd`, `cogs_usd`, and `refund_amount_usd` were checked as numeric fields.
   - Descriptive fields such as `product_name`, `utm_source`, `utm_campaign`, and `device_type` were kept as text fields.

3. **Date-only columns**
   - A date-only column named `created_date` was created from `created_at` for the transactional tables.
   - This avoids relationship issues caused by joining a date table to date/time values that include hours, minutes, and seconds.

4. **Query naming**
   - Query names were kept close to the source file names to make the data lineage clear.

---

## Date Handling

The original `created_at` fields contain timestamp values. They are useful for detailed event analysis, but they are not ideal for direct relationship to a date table.

For date-based analysis, each relevant transactional table uses a `created_date` field.

Example:

```text
created_at   = 2014-11-28 13:45:00
created_date = 2014-11-28
```

---

## Data Quality Notes

Some values should be reviewed before the final portfolio version:

| Field | Current Issue | Suggested Cleanup |
|---|---|---|
| `utm_source` | May contain blank or `NULL` traffic source values | Replace with `Direct / None` if appropriate |
| `utm_campaign` | May contain blank or `NULL` campaign values | Replace with `No Campaign` if appropriate |
| `device_type` | May show `--` values | Replace with `Unknown` if appropriate |
| Year slicer | May show blank or `--` if date relationships are incomplete | Validate `DimDate` coverage and filter blank year values |

These cleanup decisions should be applied only if they do not distort the source meaning.

---

## Power Query Scope

The current project uses Power Query primarily for:

- importing CSV tables,
- promoting headers,
- setting data types,
- creating date-only fields,
- keeping table names clean and traceable.

Most business logic is handled later through the data model and DAX measures.