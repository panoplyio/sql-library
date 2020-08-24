# Top 20 Worsened Queries Week on Week

Instructions | Details
---|---
Description | Display the top 20 queries with the most significant decrease in average position over the last week.
Requirements | Collect the Panoply Google Search Console data source with the default set of metrics and dimensions. Dimensions used: `date`. Metrics used: `query`, `position`, and `impressions`.
Usage | Create a table or bar chart to see which queries have worsened the most week on week.
Modifications | <ul><li><b>Custom date range:</b> In all `WHERE` clauses, change `'7 days'` to `'14 days'` and `'14 days'` to `'28 days'` to see the worst performing queries fortnight on fortnight.</li> <li><b>Custom number of queries: </b> Change the `ORDER BY` clause from `LIMIT 20` to `LIMIT N` to see the top N worst performing queries week on week, for some whole number N.</li></ul>

```sql
SELECT 
  last7.query,
  last7.avg_pos AS last_7_avg_pos,
  prev7.avg_pos AS prev_7_avg_pos,
  prev7.avg_pos - last7.avg_pos AS difference
FROM
  (SELECT 
    query,
    SUM(position * impressions) / SUM(impressions) AS avg_pos
   FROM google_search_console_blog 
   WHERE date < current_date -- This is the 'last 7' days
     AND date >= current_date - interval '7 days'
   GROUP BY query) AS last7
INNER JOIN
  (SELECT 
    query,
    SUM(position * impressions) / SUM(impressions) AS avg_pos
   FROM google_search_console_blog 

   WHERE date < current_date - interval '7 days' -- This is the 'previous 7' days
     AND date >= current_date - interval '14 days'
   GROUP BY query) AS prev7
  ON last7.query = prev7.query
ORDER BY difference ASC LIMIT 20;
```

## Query Results Dictionary
Column | Description
---|---
`query`| The search term typed into Google that your page(s) have ranked for.
`last_7_avg_pos`| The average position for that query over the last seven days.
`prev_7_avg_pos`| The average position for that query over the previous seven days.
`difference`| The change in average position week on week. A negative number means a decrease in position and that the query ranks further away from #1. For example, if a page ranked #5 in the previous week and #40 last week, the difference is 5 - 40 = -35. Thus the page has decreased its position by 35.
