# Top 20 Worsened Queries Week on Week

Instructions | Details
---|---
Description | Display the top 20 queries with the most significant decrease in average position week on week.
Requirements | Collect the Panoply Google Search Console data source with the default set of metrics and dimensions. Dimensions used: `date`. Metrics used: `query`, `position`, and `impressions`.
Usage | Create a table or bar chart to see which queries have worsened the most week on week.
Modifications | <ul><li><b>Custom date range:</b> In all `WHERE` clauses, change `'7 days'` to `'14 days'` and `'14 days'` to `'28 days'` to see the worst performing queries fortnight on fortnight.</li> <li><b>Custom number of queries: </b> Change the `ORDER BY` clause from `LIMIT 20` to `LIMIT N` to see the top N worst performing queries week on week, for some whole number N.</li></ul>

```sql
SELECT 
  this_week.query,
  this_week.avg_pos AS this_week_avg_pos,
  last_week.avg_pos AS last_week_avg_pos,
  last_week.avg_pos - this_week.avg_pos AS difference
FROM
  (SELECT 
    query,
    SUM(position * impressions) / SUM(impressions) AS avg_pos
   FROM google_search_console_blog 
   WHERE date < current_date
     AND date >= current_date - interval '7 days'
   GROUP BY query) AS this_week
INNER JOIN
  (SELECT 
    query,
    SUM(position * impressions) / SUM(impressions) AS avg_pos
   FROM google_search_console_blog 
   WHERE date < current_date - interval '7 days' 
     AND date >= current_date - interval '14 days'
   GROUP BY query) AS last_week
  ON this_week.query = last_week.query
ORDER BY difference ASC LIMIT 20;
```

## Query Results Dictionary
Column | Description
---|---
`query`| The search term typed into Google that your page(s) have ranked for.
`this_week_avg_pos`| The average position for that query this week.
`last_week_avg_pos`| The average position for that query last week.
`difference`| The change in average position week on week. A negative number means a decrease in position and that the query ranks further away from #1. For example, if a page ranked #5 last week and #40 this week, the difference is 5 - 40 = -35. Thus, the page has decreased its position by 35.
