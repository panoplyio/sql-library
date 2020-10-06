# Top 20 Most Improved Queries Week on Week

Instructions | Details
---|---
Description | Display the top 20 queries with the most significant increase in average position over the last week.
Requirements | Collect the Panoply Google Search Console data source with the default set of metrics and dimensions. Dimensions used: `date`. Metrics used: `query`, `position`, and `impressions`.
Usage | Create a table or bar chart to see which queries have improved the most week on week.
Modifications | <ul><li><b>Custom date range:</b> In all `WHERE` clauses, change `'7 days'` to `'14 days'` and `'14 days'` to `'28 days'` to see the best performing queries fortnight on fortnight.</li> <li><b>Custom number of queries: </b> Change the `ORDER BY` clause from `LIMIT 20` to `LIMIT N` to see the top N performing queries week on week, for some whole number N.</li></ul>

```sql
SELECT 
	this_week.query AS query,
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
ORDER BY difference DESC LIMIT 20;
```

## Query Results Dictionary
Column | Description
---|---
`query`| The search term typed into Google that your page(s) have ranked for.
`this_week_avg_pos`| The average position for that query this week.
`last_week_avg_pos`| The average position for that query last week.
`difference`| The change in average position week on week. A positive number means an increase in position and that the query ranks closer to #1. For example, if a page ranked #40 last week and #5 this week, the difference is 40 - 5 = 35. Thus the page has increased its position by 35.
