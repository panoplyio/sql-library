---
title: Number of Contacts by City
description: This query counts how many contacts are from each city.
usage: This query can be used to create histogram of contacts in from each city.
modifications: Add filters on the number of contacts in each city using HAVING statement.
---

```sql
SELECT CASE WHEN city IS NULL THEN 'Unspecified' ELSE city END AS "City"
	, count(contact_id) AS count_contacts
FROM hubspot_contacts_consolidated
GROUP BY 1
ORDER BY 2 DESC;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `City`| The contact's city. |
| `count_contacts`| The number of Hubspot contacts that fall into the city groupings. |
