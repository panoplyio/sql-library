# Average Search Volume - Last 4 Weeks

Instructions | Details
---|---
Description | Displays the average amount of search traffic coming to your site each day of the week over the last four weeks.
Requirements | Collect the Panoply Google Search Console data source with the default set of metrics and dimensions. Dimensions used: `date`. Metrics used: `clicks` and `impressions`.
Usage | Create a bar chart showing which days bring the most traffic. Highlight the worst-performing days and set aside for more in-depth analysis.
Modifications | <ul> <li><b>Change date range:</b> In the WHERE clause, change `'4 weeks'` to `'N weeks'` to get the average search volume for the last N weeks (for some whole number N)</li><li><b>Any daily total/average:</b> Aggregate different columns to get different daily totals/averages e.g. `SUM(clicks)` gives you the total number of clicks your site has received on each day over the last 4 weeks.</li><li><b>Alternate day ordering: </b>If you want the week to begin on Sunday, change the first column in the `SELECT` statement to `TO_CHAR(date, 'D')`. The current form is `'ID'` which stands for 'International Date'. This is the ISO 8601 international date time format which has weeks begin on Monday.</li></ul>

```sql
SELECT 
    TO_CHAR(date, 'ID') AS day_number,
    TO_CHAR(date, 'Day') AS day_of_week,
    AVG(clicks*1.00/impressions) AS avg_search_vol
FROM google_search_console_blog 
WHERE date >= current_date - interval '4 weeks' 
GROUP BY 
	day_number,
	day_of_week
ORDER BY day_number
```

## Query Results Dictionary
Column | Description
---|---
`day_number`| The day of the week as a number. Day 1 corresponds to Monday. We need this to order the results correctly (otherwise, the results would be ordered alphabetically).
`day_of_week`| The day of the week.
`avg_search_vol`| The average amount of traffic brought to your website through search on each day of the week.
