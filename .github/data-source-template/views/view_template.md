---
title: Short but Descriptive Title Without the Source Name
description: Brief summary of the view
usage: When and why to use this view
modifications: List ways this view could be modified for custom use
---

```sql
-- CREATE VIEW view_name AS
SELECT *
FROM some_table t
JOIN some_other_table ot ON t.email=ot.email
```

## Example: {Name of example}

```sql
SELECT *
FROM view_name
WHERE email not ilike '%gmail%';
```
