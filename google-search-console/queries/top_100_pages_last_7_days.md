# Top 100 Pages in Last 7 Days

Instructions | Details
---|---
Description | Display the top 100 most clicked pages on your site in the last seven days. Plus, other useful statistics.
Requirements | Collect the Panoply Google Search Console data source with the default set of metrics and dimensions. Dimensions used: `date` and `page`. Metrics used: `clicks`, `ctr`, `position`, and `impressions`.
Usage | Create a table to see the most fundamental statistics for the top-performing pages on your site. Track these statistics over time using a bar or line chart.
Modifications | <ul><li> <b>Custom date range:</b> In the `WHERE` clause, change `'7 days`' to `'N days'` to display the top 100 pages in the last N days, for some whole number N.</li>  <li> <b>Custom number of pages:</b> In the `ORDER BY` clause, change `LIMIT 100` to `LIMIT N` to show the top N pages, for some whole number N.</li> <li><b>Simplified view:</b> To just see the pages and the total number of clicks, only include the first two colums in the `SELECT` statement.</li> </ul>


```sql
SELECT 
	page,
    SUM(clicks) AS total_clicks,
    SUM(impressions) AS total_impressions,
    SUM(ctr * impressions) / SUM(impressions) AS weighted_average_ctr,
    SUM(position * impressions) / SUM(impressions) AS weighted_average_rank,
    SUM(impressions - clicks) AS missed_opportunity
FROM google_search_console_blog
WHERE date > current_date - interval '7 days'
GROUP BY page
ORDER BY total_clicks DESC LIMIT 100;
```

## Query Results Dictionary
Column | Description
---|---
`page`| The page on your site appearing in search results.
`total_clicks`| The number times this page was clicked in the last seven days.
`total_impressions` | The number of times this page was displayed in Google search results over the last seven days.
`weighted_average_ctr` | The average click-through rate for this page in the last seven days.
`weighted_average_rank`| The average position this page appeared in the last seven days in Google's search results.
`missed_opportunity`| The number of impressions minus the number of clicks i.e., the number of times the page was seen and not clicked. Pages with a high number of missed opportunities would be excellent targets for increasing your organic search traffic (they are already ranking well, but not many people are clicking through. Why might this be?).