---
title: Last 7 Days vs Previous 7 Days
description: This query compares the sessions of the last 7 days to the previous 7 days aggregated by day. This query demonstrates the use of a concept that can be used with any dimension and metric\\s, comparing different values of the metric based on different values of the dimension.
requirements: Collect any set of metrics and dimensions with at least one date or timestamp field with the Panoply Google Analytics data source. This example uses the `datehour` dimension for the date field but it could also use `date`, or `datehourminute`. This example also requires the `sessions` metric.
usage: This query can be used to create a bar chart comparing the `last_7` and `previous_7` for each day of the week.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. Other dimensions and metrics can be added to the `WHERE` as filters and other aggregations can be added on top of `last_7` and `previous_7`.
---

```sql
SELECT
  TO_CHAR(datehour, 'Day') day_of_week,
  SUM(
    CASE WHEN datehour >= current_date - 7
    AND datehour < current_date THEN sessions END
  ) last_7,
  SUM(
    CASE WHEN datehour >= current_date - 14
    AND datehour < current_date - 7 THEN sessions END
  ) previous_7
FROM
  public."google-analytics" -- Table name might be different based on Schema and Destination settings in the data source
WHERE
  datehour >= current_date - 14
  AND datehour < current_date
GROUP BY
  1
ORDER BY
  MIN(
    CASE WHEN datehour BETWEEN current_date - 7
    AND current_date - 1 THEN datehour END
  ) ASC
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `day_of_week`| Day of the week extracted from datehour |
| `last_7`| Values for last 7 days |
| `previous_7`| Values for previous 7 days |
