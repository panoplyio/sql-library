---
title: Owners consolidated
description: Denormalized view of Hubspot's owners data. This view can be used together with other Hubspot views
usage: This is the "ready for analysis" view of Hubspot owners
modifications: Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name. Dates are formatted as Unix timestamps so they must be transformed and cast into dates.
---

```sql
-- CREATE VIEW hubspot_owners_consolidated AS
SELECT ownerid AS owner_id
	, type
	, firstname AS first_name
	, lastname AS last_name
	, email
	, TIMESTAMP 'epoch' + createdat / 1000 * interval '1 second' AS created_date
	, TIMESTAMP 'epoch' + updatedat / 1000 * interval '1 second' AS updated_date
FROM hubspot_owners
WHERE case when isactive = 1 then TRUE when isactive = 0 then FALSE else isactive::bigint::boolean end IS TRUE;
```
