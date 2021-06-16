---
title: Expected Revenue by Close Month
description: This query calculates the total forecasted revenue per month. It does not include deals that were either closed won or closed lost.
usage: This query can be used to create a line or bar chart of forecasted revenue per month.
modifications: -
---

```sql
SELECT date_part(YEAR, close_date) AS "Close Year"
	, date_part(MONTH, close_date) AS "Close Month"
	, sum(forecast_amount) AS "Pipeline - Forecasted Amount"
FROM hubspot_deals_consolidated
WHERE deal_stage NOT IN ('closedwon', 'closedlost')
GROUP BY 1, 2
ORDER BY 1, 2;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `Close Year`| Year of closing date. |
| `Close Month"`| Month of closing date. |
| `Pipeline - Forecasted Amount"`| The total forecasted revenue. |
