---
title: Monthly Performance Breakdown
description: This query sums the total cost and conversions by month and year from the `adgroup_performance_report`.
requirements: Collect the `Adgroup Performance Report` report with the Panoply Adwords API data source.
usage: This query can be used to create a line or bar chart of cost and conversions over time.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The columns `ad group` or `campaign` can be added to the `WHERE` as filters or to the `SELECT` and `GROUP BY` for an additional level of aggregation granularity.
---

```sql
SELECT
  DATE_TRUNC('month', "day") :: date year_month,
  ROUND(SUM(cost :: float / 1000000), 2) cost, -- divide cost by 1000000 to get Dollar since Google Provide Micro Dollar units  - Link to Google Adwords Docs https://developers.google.com/adwords/api/docs/appendix/reports/adgroup-performance-report#cost
  SUM(conversions):: bigint conversions,
  SUM(clicks) clicks,
  SUM(impressions) impressions,
  SUM(clicks :: float)/ SUM(impressions) clickthrough_rate,
  SUM(cost :: float / 1000000)/ SUM(clicks) cost_per_click
FROM
  public.adwords_adgroup_performance_report -- Table name might be different based on Schema and Destination settings in the data source
GROUP BY
  1
ORDER BY
  1 ASC
```

## Query Results Dictionary
| Column | Description |
| --- | --- |
| `year_month`| Year and month extracted from the day column. Values are in date format with the first day of each month to represent that given month. |
| `cost`| Total monthly cost in Dollars |
| `conversions`| Total monthly conversions |
| `clicks`| Total monthly clicks |
| `impressions`| Total monthly impressions |
| `clickthrough_rate`| Monthly clicks divided by monthly impressions |
| `cost_per_click`| Total monthly cost divided by monthly clicks |
