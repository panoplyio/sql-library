---
title: Number of contacts by Source
description: This query counts how many contacts came from each source.
usage: This query can be used to create histogram of contacts in from each source.
modifications: Add filters on the number of contacts in each source using HAVING statement.
---

```sql
SELECT source
	, count(contact_id) count_contacts
FROM hubspot_contacts_consolidated
GROUP BY 1
ORDER BY 2 DESC;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `source`| The source from which the contacts came. |
| `count_contacts`| The number of Hubspot contacts that fall into the source groupings. |
