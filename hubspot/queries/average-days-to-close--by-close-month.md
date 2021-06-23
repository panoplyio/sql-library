---
title: Average Days to Close by Close Month
description: This query calculates the average number of days to close (total, won and lost deals) per month. It does not include deals that were either closed won or closed lost.
usage: This query can be used to create a line or bar chart of average days to close (three different metrics) per month.
---

```sql
SELECT date_part(YEAR, close_date) AS "Close Year"
	, date_part(MONTH, close_date) AS "Close Month"
	, avg(days_to_close) AS "Average Days to Close"
	, avg(CASE WHEN date_entered_closedwon IS NOT NULL THEN cast(days_to_close AS INT) ELSE 0 END) AS "Average Days to Closed Won"
	, avg(CASE WHEN date_entered_closedlost IS NOT NULL THEN cast(days_to_close AS INT) ELSE 0 END) AS "Average Days to Closed Lost"
FROM hubspot_deals_consolidated
WHERE date_entered_closedwon IS NOT NULL OR date_entered_closedlost IS NOT NULL
GROUP BY 1, 2
ORDER BY 1, 2;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `Close Year`| Year of closing date. |
| `Close Month`| Month of closing date. |
| `Average Days to Close`| The average amount of days to close a deal. |
| `Average Days to Closed Won`| The average amount of days to win a deal. |
| `Average Days to Closed Lost`| The average amount of days to lose a deal. |
