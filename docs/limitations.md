# Limitations and Assumptions

## Dataset Limitations

### 1. Sample dataset

This project uses a sample e-commerce dataset. The analysis is suitable for Power BI practice and portfolio demonstration, but it should not be treated as a real company's live business performance.

### 2. Limited product catalog

The dataset contains a small number of products. Product-level charts are useful for learning and demonstration, but they do not represent the complexity of a large product catalog.

### 3. No country or region field

The dataset does not support country or region analysis. For that reason, the original country analysis requirement was replaced with Marketing Channel Analysis.

The replacement dimension is based on:

- `utm_source`,
- `utm_campaign`,
- `device_type`.

### 4. No advertising spend data

The dataset includes marketing source and campaign fields, but it does not include campaign cost or advertising spend.

Because of this, the dashboard can analyze conversion, orders, and revenue by channel, but it cannot calculate true return on ad spend.

### 5. No customer demographic data

The dataset includes `user_id`, but it does not provide demographic information such as age, gender, location, or customer segment.

Customer segmentation is therefore outside the scope of this project.

### 6. No predictive modeling

This project focuses on descriptive and diagnostic analytics in Power BI. It does not attempt to build a predictive model, forecast future sales, or classify customer behavior.

---

## Data Quality Assumptions

### 1. Blank and `NULL` values

Some marketing fields may contain blank or `NULL` values. These values are interpreted as unattributed or direct traffic unless further source documentation says otherwise.

Recommended final label:

```text
Direct / None
```

### 2. Unknown device values

Some device fields may show `--`. These values should be treated as unknown device type unless source documentation provides a better definition.

Recommended final label:

```text
Unknown
```

### 3. Date analysis

Date-based analysis uses date-only fields derived from the original `created_at` timestamp values.

This is necessary because joining a date table to a date/time field can cause incorrect one-to-one relationships or unmatched date values.

### 4. Refund relationship

Refund analysis assumes that refunds can be related to products through:

```text
products -> order_items -> order_item_refunds
```

This relationship path must be validated before final conclusions are written about product-level refund performance.

---

## Modeling Assumptions

### 1. Sessions as funnel starting point

The model treats `website_sessions` as the beginning of the e-commerce funnel. This means traffic and conversion analysis starts from sessions.

### 2. Orders connected to sessions

The model assumes that orders can be connected back to the session that generated the order through `website_session_id`.

### 3. Product revenue from order items

Product-level revenue and cost analysis should use `order_items`, not only `orders`, because `order_items` stores item-level product information.

### 4. Country requirement replacement

Because the dataset does not contain country fields, the capstone requirement for Country Analysis and RLS by Country is replaced with:

```text
Marketing Channel Analysis
RLS by UTM Source
```

This keeps the analytical requirement aligned with the actual data.

---

## Report Limitations

### 1. Draft visual layout

The current dashboard pages are functional but still need final visual polish for portfolio presentation.

Recommended improvements:

- reduce thick visual borders,
- align KPI cards consistently,
- replace technical field names in visual titles,
- improve tooltip readability,
- add final dashboard screenshots.

### 2. Tooltip size

The tooltip page may become difficult to read if it contains too many visuals. A tooltip should remain compact and focused.

### 3. Bookmark maintenance

Bookmark interactions depend on visual visibility states. If visuals are added, removed, or renamed, bookmarks should be updated and tested again.

### 4. RLS validation

RLS roles based on `utm_source` should be tested using Power BI's **View as** feature before publishing.

Recommended roles:

- `Role_GSearch`,
- `Role_BSearch`,
- `Role_Socialbook`,
- `Role_Direct`.

Avoid keeping a role named only `null` in the final portfolio version. Replace it with a clearer business label if blank traffic source values are retained.

---

## Final Review Checklist

Before considering the project final, check:

1. DAX measures return expected results under filters.
2. Refund metrics respond correctly to product filters.
3. Slicers do not show confusing blank values.
4. All report titles use business-friendly names.
5. Drillthrough works from Product Analysis to Product Drillthrough.
6. Tooltip appears correctly on product visuals.
7. Bookmark buttons correctly switch between revenue and conversion views.
8. RLS roles filter report data correctly.
9. README includes final dashboard preview screenshots.
10. Final recommendations are based on validated metrics.