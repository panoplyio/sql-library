---
title: Salesforce: All revenue and loss for the Quarter-To-Date (QTD) grouped by type of opportunity.
description: This query sums the total amount from opportunities grouped by type. A total is displayed at the bottom
requirements: Collect the `Opportunities` Resource with the Panoply Salesforce data source.
usage: This query can be displayed in a pivot form to display the total amount per opportunity type.

modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Date Range Filter using the `closedate` in the `WHERE` clause can be changed.
---

# Salesforce: All revenue and loss for the Quarter-To-Date (QTD) grouped by type of opportunity.

```sql
SELECT
    "type",
    SUM("amount") as "amount"
FROM
    public.salesforce_opportunity
WHERE
    EXTRACT(quarter FROM "closedate") = EXTRACT(quarter FROM CURRENT_DATE)
  AND stagename = 'Closed Won'
GROUP BY 1
UNION ALL
SELECT
    'TOTAL AMOUNT:' as "type",
    SUM("amount") as amount
FROM
    public.salesforce_opportunity
WHERE
    EXTRACT(quarter FROM "closedate") = EXTRACT(quarter FROM CURRENT_DATE)
  AND stagename = 'Closed Won'
order by amount
```

## Query Results Dictionary
Column | Description
---|---
`type`| Opportunity type
`amount`| Sum of the opportunity amount
