---
title: Deals Companies
description: One to many connection table between all the companies to their respective deals
usage: This view can be used in order to connect between a company and its deals
modifications: -
---

```sql
-- CREATE VIEW hubspot_deals_companies AS
SELECT hda.hubspot_deals_id AS deal_id
	, hdaac.value AS company_id
FROM hubspot_deals_associations hda
LEFT JOIN hubspot_deals_associations_associatedcompanyids hdaac ON hda.id = hdaac.hubspot_deals_associations_id
WHERE hdaac.value IS NOT NULL
GROUP BY 1, 2;
```
