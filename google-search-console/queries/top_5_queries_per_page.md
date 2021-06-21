# Top 5 Most Clicked Queries Per Page In The Last 28 days (Top 100 Pages)

Instructions | Details
---|---
Description | For each of the top 100 pages on your site, this query finds the top 5 most clicked queries, and the number of clicks they generated in the last 28 days.
Requirements | Collect the Panoply Google Search Console data source with the default set of metrics and dimensions. Dimensions used: `date`, `page`, `query`. Metrics used: `clicks`.
Usage | Create a table to see which keywords rank highly and bring in traffic for each page.
Modifications | <ul><li><b>Custom number of queries: </b>In the final `FROM` statement, change `WHERE rank <= 5` to `WHERE rank <= N` to get the top N best performing queries per page, for some whole number N.</li> <li><b>Custom date range: </b>Change the first `WHERE` clause to `WHERE date > current_date - interval 'N days'` to get the top 5 best performing queries in the last N days, for some whole number N.</li><li><b>Worst performing queries: </b>Get the top 100 worst performing queries and pages by changing the ORDER BY clause to `ORDER BY total_clicks ASC`.</li></ul>

```sql
WITH page_info AS(
    SELECT
        page,
        query,
        SUM(clicks) AS total_clicks,
        ROW_NUMBER() OVER(PARTITION BY page ORDER BY total_clicks DESC) AS rank
    FROM google_search_console_blog
    WHERE date > current_date - interval '28 days'
    GROUP BY 1, 2
    ORDER BY page, total_clicks DESC)
---------------------
SELECT DISTINCT
	page,
	LISTAGG(query, ', ') OVER(PARTITION BY page) AS top_5_queries,
	SUM(total_clicks) OVER(PARTITION BY page) AS total_clicks
FROM (SELECT *
	  FROM page_info
	  WHERE rank <= 5)
ORDER BY total_clicks DESC
LIMIT 100;
```

## Query Results Dictionary
Column | Description
---|---
`page`| The page on your site appearing in search results.
`top_5_queries`| The five queries that have generated the most clicks for this page in the last 28 days.
`total_clicks`| The sum of all the clicks that the five queries generated for this page in the previous 28 days. 
