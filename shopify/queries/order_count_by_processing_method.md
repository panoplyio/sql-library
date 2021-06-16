---
title: Order Analytics by Processing Method
description: This query shows an overview of how many orders by financial status there are per processing method.
requirements: Collect the `Orders` Resource with the Panoply Shopify data source.
usage: This query can be displayed in a pivot form to display how many orders by financial status there are per processing method.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Date Range Filter using the `created_at` in the `WHERE` clause can be changed.
---

```sql
SELECT
  so.processing_method,
  COUNT(CASE WHEN so.financial_status = 'pending' THEN 1 END) AS "pending",
  COUNT(CASE WHEN so.financial_status = 'authorized' THEN 1 END) AS "authorized",
  COUNT(CASE WHEN so.financial_status = 'partially_paid' THEN 1 END) AS "partially_paid",
  COUNT(CASE WHEN so.financial_status = 'paid' THEN 1 END) AS "paid",
  COUNT(CASE WHEN so.financial_status = 'partially_refunded' THEN 1 END) AS "partially_refunded",
  COUNT(CASE WHEN so.financial_status = 'refunded' THEN 1 END) AS "refunded",
  COUNT(CASE WHEN so.financial_status = 'voided' THEN 1 END) AS "voided",
  COUNT(*) AS total_orders
FROM
  public.shopify_orders so
WHERE
  DATE_TRUNC('quarter', so."created_at") = DATE_TRUNC('quarter', CURRENT_DATE)
GROUP BY
  1
ORDER BY
  total_orders DESC
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `processing_method`| Processing methods used by the orders |
| `pending`| Order Count with the financial status "pending" |
| `authorized`| Order Count with the financial status "authorized" |
| `partially_paid`| Order Count with the financial status "partially_paid" |
| `paid`| Order Count with the financial status "paid" |
| `partially_refunded`| Order Count with the financial status "partially_refunded" |
| `refunded`| Order Count with the financial status "refunded" |
| `voided`| Order Count with the financial status "voided" |
