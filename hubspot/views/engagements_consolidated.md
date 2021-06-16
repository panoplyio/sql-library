---
title: Engagements consolidated
description: Denormalized view of Hubspot's engagements data. This view can be used together with other Hubspot views
usage: This is the "ready for analysis" view of Hubspot engagements
modifications: Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name.
---

```sql
-- CREATE VIEW hubspot_engagements_consolidated AS
SELECT e.id AS engagement_id
	, e.ownerid AS owner_id
	, ecp.value AS company_id
	, ect.value AS contact_id
	, ed.value AS deal_id
	, e."type"
	, e."timestamp" AS engagement_timestamp
FROM hubspot_engagements e
LEFT JOIN hubspot_engagements_associations_companyids ecp ON e.id = ecp.hubspot_engagements_id
LEFT JOIN hubspot_engagements_associations_contactids ect ON e.id = ect.hubspot_engagements_id
LEFT JOIN hubspot_engagements_associations_dealids ed ON e.id = ed.hubspot_engagements_id
WHERE case when e.active = 1 then TRUE when e.active = 0 then FALSE else e.active::bigint::boolean end IS TRUE;
```
