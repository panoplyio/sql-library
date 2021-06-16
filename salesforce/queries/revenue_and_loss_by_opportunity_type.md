---
title: All revenue and loss for the Quarter-To-Date (QTD) grouped by type of opportunity.
description: This query sums the total amount from opportunities grouped by type. A total is displayed at the bottom
requirements: Collect the `Opportunities` Resource with the Panoply Salesforce data source.
usage: This query can be displayed in a pivot form to display the total amount per opportunity type.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Date Range Filter using the `closedate` in the `WHERE` clause can be changed. Everything below the UNION ALL clause can also be removed if you prefer to not have the total at the bottom.
---

```sql
SELECT
  "type",
  SUM("amount") AS "amount",
  COUNT(*) AS "opps"
FROM
  public.salesforce_opportunity
WHERE
  DATE_TRUNC('quarter', "closedate") = DATE_TRUNC('quarter', CURRENT_DATE)
  AND stagename = 'Closed Won'
GROUP BY
  1
UNION ALL
SELECT
  'Total' AS "type",
  SUM("amount") AS "amount",
  COUNT(*) AS "opps"
FROM
  public.salesforce_opportunity
WHERE
  DATE_TRUNC('quarter', "closedate") = DATE_TRUNC('quarter', CURRENT_DATE)
  AND stagename = 'Closed Won'
ORDER BY
  amount
```

## Query Results Dictionary

| Column | Description |
|--- | --- |
| `type` | Opportunity type |
| `amount` | Sum of the opportunity amount |
| `opps` | Opportunity count |
