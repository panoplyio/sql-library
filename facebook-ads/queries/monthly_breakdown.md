# ﻿Monthly Breakdown

Instructions | Details
---|---
Description | This query sums the total cost, clicks and impressions by month and year from  `facebook-ads`.
Requirements | Collect data with the Panoply Facebook Ads data source.
Usage | This query can be used to create a line or bar chart of cost \ impressions \ clicks over time.
Modifications | The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The columns `campaign_name` or your chosen breakdown (`country` by default) can be added to the `WHERE` as filters or to the `SELECT` and `GROUP BY` for an additional level of aggregation granularity.

```sql
SELECT
  EXTRACT(YEAR FROM date_start) "year",
  EXTRACT(MONTH FROM date_start) "month",
  ROUND(SUM(spend), 2) cost,
  SUM(clicks) clicks,
  SUM(impressions) impressions,
  ROUND(SUM(clicks :: float)/ SUM(impressions), 3) clickthrough_rate,
  ROUND(SUM(spend :: float)/ SUM(clicks), 2) cost_per_click
FROM
  public."facebook-ads" -- Table name might be different based on Schema and Destination settings in the data source
GROUP BY
  1,
  2
ORDER BY
  1 ASC,
  2 ASC
```

## Query Results Dictionary
Column | Description
---|---
`year`| Year extracted from the date_start column
`month`| Month extracted from the date_start column
`cost`| Total monthly cost in Dollars
`clicks`| Total monthly clicks
`impressions`| Total monthly impressions
`clickthrough_rate`| Monthly clicks divided by monthly impressions
`cost_per_click`| Total monthly cost divided by monthly clicks
