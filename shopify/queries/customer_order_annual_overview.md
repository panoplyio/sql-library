---
title: Shopify: Customer Order Annual Overview.
description: This query shows an overview of a customer's orders by processing method, financial status, discount codes, and total discount derived from Shopify data.
requirements: Collect the `Orders` Resource with the Panoply Shopify data source. This will create sub-tables for the `Customer` and `Discount Codes` data.
usage: This query can be displayed in a tabular form to display the order pattern of the customer.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Date Range Filter using the `created_at` in the `WHERE` clause can be changed.
---

# Shopify: Customer Order Annual Overview.

```sql
SELECT
  soc.first_name || ' ' || soc.last_name AS "customer",
  soc.email,
  SUM(so.total_discounts) AS "total_discounts",
  LISTAGG(distinct sodc.code, ', ') AS "discount_codes",
  COUNT(*) AS "total_orders",
  COUNT(CASE WHEN so.processing_method = 'checkout' THEN 1 END) AS "checkout",
  COUNT(CASE WHEN so.processing_method = 'direct' THEN 1 END) AS "direct",
  COUNT(CASE WHEN so.processing_method = 'manual' THEN 1 END) AS "manual",
  COUNT(CASE WHEN so.processing_method = 'offsite' THEN 1 END) AS "offsite",
  COUNT(CASE WHEN so.processing_method = 'express' THEN 1 END) AS "express",
  COUNT(CASE WHEN so.processing_method = 'free' THEN 1 END) AS "free",
  COUNT(CASE WHEN so.financial_status = 'pending' THEN 1 END) AS "pending",
  COUNT(CASE WHEN so.financial_status = 'authorized' THEN 1 END) AS "authorized",
  COUNT(CASE WHEN so.financial_status = 'partially_paid' THEN 1 END) AS "partially_paid",
  COUNT(CASE WHEN so.financial_status = 'paid' THEN 1 END) AS "paid",
  COUNT(CASE WHEN so.financial_status = 'partially_refunded' THEN 1 END) AS "partially_refunded",
  COUNT(CASE WHEN so.financial_status = 'refunded' THEN 1 END) AS "refunded",
  COUNT(CASE WHEN so.financial_status = 'voided' THEN 1 END) AS "voided"
FROM
  public.shopify_orders so
LEFT JOIN
  public.shopify_orders_customer soc
  ON soc.shopify_orders_id = so.id
LEFT JOIN
  public.shopify_orders_discount_codes sodc
  ON sodc.shopify_orders_id = so.id
WHERE
  DATE_TRUNC('year', so."created_at") = DATE_TRUNC('year', CURRENT_DATE)
  AND customer IS NOT NULL
  AND soc.email IS NOT NULL
GROUP BY
    1,
    2
ORDER BY
    total_orders DESC
```

## Query Results Dictionary
Column | Description
---|---
`customer`| Customer's Full Name
`email`| Customer's email
`total_discounts`| total discount amount from all orders
`discount_codes`| comma-separated list of all discount codes used
`total_orders`| total order count
`checkout`| Order Count under the processing method "checkout"
`direct`| Order Count under the processing method "direct"
`manual`| Order Count under the processing method "manual"
`offsite`| Order Count under the processing method "offsite"
`express`| Order Count under the processing method "express"
`free`| Order Count with the financial status "free"
`pending`| Order Count with the financial status "pending"
`authorized`| Order Count with the financial status "authorized"
`partially_paid`| Order Count with the financial status "partially_paid"
`paid`| Order Count with the financial status "paid"
`partially_refunded`| Order Count with the financial status "partially_refunded"
`refunded`| Order Count with the financial status "refunded"
`voided`| Order Count with the financial status "voided"
