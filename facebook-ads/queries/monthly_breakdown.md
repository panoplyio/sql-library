--
title: Facebook Ads - Monthly Breakdown
description: This query sums the total cost, clicks and impressions by month and year from `facebook-ads`.
requirements: Collect data with the Panoply Facebook Ads data source.
usage: This query can be used to create a line or bar chart of cost \ impressions \ clicks over time.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The columns `campaign_name` or your chosen breakdown (`country` by default) can be added to the `WHERE` as filters or to the `SELECT` and `GROUP BY` for an additional level of aggregation granularity.
---

# Facebook Ads - Monthly Breakdown

```sql
SELECT
  DATE_TRUNC('month', date_start) :: date year_month,
  ROUND(SUM(spend), 2) cost,
  SUM(clicks) clicks,
  SUM(impressions) impressions,
  ROUND(SUM(clicks :: float)/ SUM(impressions), 3) clickthrough_rate,
  ROUND(SUM(spend :: float)/ SUM(clicks), 2) cost_per_click
FROM
  public."facebook-ads" -- Table name might be different based on Schema and Destination settings in the data source
GROUP BY
  1
ORDER BY
  1 ASC
```

## Query Results Dictionary
Column | Description
---|---
`year_month`| Year and month extracted from the date_start column. Values are in date format with the first day of each month to represent that given month.
`cost`| Total monthly cost in the ad account's chosen currency
`clicks`| Total monthly clicks
`impressions`| Total monthly impressions
`clickthrough_rate`| Monthly clicks divided by monthly impressions
`cost_per_click`| Total monthly cost divided by monthly clicks
