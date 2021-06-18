---
title: Deals Contacts
description: Many to many connection table between all the contacts to their respective deals
usage: This view can be used in order to connect between a contact and its deals
modifications: -
---

```sql
-- CREATE VIEW hubspot_deals_contacts AS
SELECT hda.hubspot_deals_id AS deal_id
	, hdaav.value AS contact_id
FROM hubspot_deals_associations hda
LEFT JOIN hubspot_deals_associations_associatedvids hdaav ON hda.id = hdaav.hubspot_deals_associations_id
WHERE hdaav.value IS NOT NULL
GROUP BY 1, 2;
```
