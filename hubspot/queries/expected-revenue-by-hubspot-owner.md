---
title: Expected Revenue by Hubspot Owner
description: This query calculates the total forecasted revenue per Hubspot owner. It does not include deals that were either closed won or closed lost.
usage: This query can be used to create histogram of sales representatives and their forecasted revenue.
---

```sql
SELECT o.owner_id
	, o.first_name
	, o.last_name
	, sum(d.forecast_amount) AS "Pipeline - Forecasted Amount"
FROM hubspot_deals_consolidated d
INNER JOIN hubspot_owners_consolidated o ON d.owner_id = o.owner_id
WHERE deal_stage NOT IN ('closedwon', 'closedlost')
GROUP BY 1, 2, 3
ORDER BY 4 DESC;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `owner_id`| The identifier of the Hubspot owner. |
| `first_name`| The first name of the Hubspot owner. |
| `last_name`| The last name of the Hubspot owner. |
| `Pipeline - Forecasted Amount`| The total forecasted revenue. |
