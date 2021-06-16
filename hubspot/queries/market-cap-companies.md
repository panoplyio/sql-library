---
title: Company Count by Market Cap
description: This query counts how many companies are in each cap size ('Small Cap', 'Mid Cap' and 'Large Cap'). This data is pulled from the [Hubspot API into Panoply](panoply.io/docs/data-sources/hubspot/).
usage: This query can be used to create histogram of companies in each cap size.
modifications: Change the bin size by altering the integers in the CASE statement and split the groupings to additional bins by adding additional conditions.
---

```sql
SELECT CASE WHEN annual_revenue < 1000000000 THEN 'Small Cap' WHEN annual_revenue < 10000000000 THEN 'Mid Cap' ELSE 'Large Cap' END AS "Cap Size"
	, count(company_id) AS "Count Companies"
FROM hubspot_companies_consolidated
GROUP BY 1;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `Cap Size`| Bin indicating the annual revenue. |
| `Count Companies`| The number of Hubspot companies that fall into the cap size groupings. |
