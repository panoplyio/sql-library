# {Name of View}

Instructions | Details
---|---
Description | Brief summary transformation
Usage | When and why to use this view
Modifications | List ways this view could be modified for custom use

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
