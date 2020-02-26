# ﻿Last 7 VS Previous 7

Instructions | Details
---|---
Description | This query compares the sessions of the last 7 days to the previous 7 days aggregated by day. This query demonstrates the use of a concept that can be used with any dimension and metric\\s, comparing different values of the metric based on different values of the dimension.
Requirements | Collect any set of metrics and dimensions with the Panoply Google Analytics data source. This example uses the `datehour` dimension with the `sessions` metric.
Usage | This query can be used to create a bar chart comparing the `last_7` abd `previous_7` for each day of the week.
Modifications | The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. Other dimensions and metrics can be added to the `WHERE` as filters and other aggregations can be added on top of `last_7` and `previous_7`.

```sql
SELECT
  TO_CHAR(datehour, 'Day') day_of_week,
  SUM(
    CASE WHEN datehour BETWEEN current_date - 7
    AND current_date - 1 THEN sessions END
  ) last_7,
  SUM(
    CASE WHEN datehour BETWEEN current_date - 14
    AND current_date - 8 THEN sessions END
  ) previous_7
FROM
  public."google-analytics" -- Table name might be different based on Schema and Destination settings in the data source
WHERE
  datehour BETWEEN current_date - 14
  AND current_date - 1
GROUP BY
  1
ORDER BY
  MIN(
    CASE WHEN datehour BETWEEN current_date - 7
    AND current_date - 1 THEN datehour END
  ) DESC
```

## Query Results Dictionary
Column | Description
---|---
`day_of_week`| Day of the week extracted from datehour, 0 = Sunday
`last_7`| Values for last 7 days
`previous_7`| Values for previous 7 days
