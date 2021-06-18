---
title: Lifetime Value and Lifetime Deal Count
description: This query calculates the lifetime value and number of deals per company
usage: This query can be used to create a list of all companies and their LTV and LTD.
modifications: Additional filters can be added, both on companies properties and/or the calculated LTV and LTD
---

```sql
SELECT cp.company_name
	, coalesce(SUM(d.closed_amount_in_home_currency), 0) AS LTV
	, coalesce(COUNT(d.deal_id), 0) AS LTD
FROM hubspot_companies_consolidated cp
LEFT JOIN hubspot_deals_companies dc ON dc.company_id = cp.company_id
INNER JOIN hubspot_deals_consolidated d ON d.deal_id = dc.deal_id
WHERE d.deal_stage = 'closedwon'
GROUP BY 1
ORDER BY 2 DESC, 3 DESC;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `company_name`| Company name. |
| `LTV`| Lifetime value. |
| `LTD`| Lifetime deals count. |
