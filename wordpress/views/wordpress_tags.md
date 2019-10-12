# WordPress Tags

Name | `wordpress_tags`
---|---
Description| A lookup table between wordpress posts and related posts tags
Usage| A DRY way to avoid joining posts to tags over and over
Modifications| Could include term_group but it is barely supported by WordPress

```sql
CREATE VIEW wordpress_tags AS (
SELECT
	object_id post_id,
	terms.term_id,
	terms.slug,
	terms.name
FROM wordpress_wp_term_relationships term_rels
JOIN wordpress_wp_terms terms on term_rels.term_taxonomy_id=terms.term_id
);
```

## Example: List tags by post id

```sql
SELECT
  post_id,
  listagg(slug, ', ') WITHIN GROUP (ORDER BY slug) tags_list
FROM wordpress_tags
GROUP BY post_id;
```
