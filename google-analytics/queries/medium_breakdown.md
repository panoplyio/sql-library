# Medium Breakdown

Instructions | Details
---|---
Description | This displays a sessions and bounce rate from the past 30 days aggregated by medium.
Requirements | Collect the Panoply Google Analytics data source with the default set of metrics and dimensions. Dimensions used: `medium` and `datehour`. Metrics used: `sessions` and `bounces`
Usage | The result of this query can be displayed as a table or also as a pie chart.
Modifications | The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The columns `country` or `devicecategory` can be added to the `WHERE` as filters or to the `SELECT` and `GROUP BY` for an additional level of aggregation granularity.

```sql
SELECT
  medium,
  SUM(sessions) sum_sessions,
  SUM(bounces):: float / SUM(sessions) bounce_rate
FROM
  public."google-analytics"
WHERE
  datehour > sysdate - 30 -- Value can be changed \ Filter can be removed
GROUP BY
  1
ORDER BY
  2 DESC
```

## Query Results Dictionary
Column | Description
---|---
`medium`| The type of referral
`sum_sessions`| Sum of sessions
`bounce_rate`| Sum of bounces divided by sum of sessions
