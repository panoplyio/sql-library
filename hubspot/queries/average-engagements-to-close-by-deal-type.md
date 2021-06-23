---
title: Average Engagements to Close by Deal Type
description: This query calculates the average number of engagements for a closed deal per deal type.
usage: This query can be used to create histogram of number of engagements in each deal type.
---

```sql
SELECT d.deal_type
	, avg(e.engagement_count) AS "Average Engagements to Close"
FROM hubspot_deals_consolidated d
INNER JOIN (
	SELECT deal_id
		, count(engagement_id) AS engagement_count
	FROM hubspot_engagements_consolidated
	GROUP BY 1
	) e ON d.deal_id = e.deal_id
WHERE d.deal_stage IN ('closedwon', 'closedlost')
GROUP BY 1
ORDER BY 2;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `deal_type`| The type of the deal. |
| `Average Engagements to Close`| The average number of engagements to close the deal. |
