# Facebook Ads - ﻿Last 7 VS Previous 7

Instructions | Details
---|---
Description | This query compares the sessions of the last 7 days to the previous 7 days aggregated by `campaign_name`. This query demonstrates the use of a concept that can be used with any dimension and metric\\s, comparing different values of the metric based on different values of time or a given dimension.
Requirements | Collect data with the Panoply Facebook Ads data source.
Usage | This query can be used to create a bar chart comparing the `last_7` and `previous_7` for each campaign or simply displayed as a table.
Modifications | The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. Other dimensions and metrics can be added as filters to the `WHERE` or by adding a `HAVING` clause and other aggregations can be added on top of `last_7` and `previous_7`. This query sums the metric `clicks`, this can be changed to a different metric like `spend` or `impressions`.

```sql
SELECT
  campaign_name,
  SUM(
    CASE WHEN date_start >= current_date - 7
    AND date_start < current_date THEN clicks END
  ) last_7,
  SUM(
    CASE WHEN date_start >= current_date - 14
    AND date_start < current_date - 7 THEN clicks END
  ) previous_7
FROM
  public."facebook-ads" -- Table name might be different based on Schema and Destination settings in the data source
WHERE
  date_start >= current_date - 14
  AND date_start < current_date
GROUP BY
  1
ORDER BY
  1
```

## Query Results Dictionary
Column | Description
---|---
`campaign_name`| Facebook Ads campaign name
`last_7`| Values for last 7 days
`previous_7`| Values for previous 7 days
