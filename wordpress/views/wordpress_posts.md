# WordPress Posts

| Name | `wordpress_posts`|
|---|---|---|
|Description|A concise view of published WordPress posts and their authors|
|Usage|Master data related to WordPress posts. Great for aggregating metrics for analytics tools like Google Analytics|
|Modifications|	To include or remove post types, modify the posts_type lists. Remove the post_status filter to include all draft versions of each posts (not recommended)|

```sql
CREATE VIEW wordpress_posts AS (
SELECT
	posts.id,
	posts.post_name,
	posts.post_title,
	users.id author_id,
	users.display_name author_display_name,
	posts.post_type,
	posts.post_status,
	posts.post_parent,
	posts.comment_status,
	posts.comment_count,
	posts.post_date_gmt,
	posts.post_modified_gmt,
	posts.guid
FROM wordpress_wp_posts posts
JOIN wordpress_wp_users users ON posts.post_author = users.id
WHERE
	post_type IN ('page','post','forum','topic')
	AND post_status = 'publish'
);
```

## Example: Post count by author

```sql
SELECT author_display_name, count(distinct id)
FROM wordpress_posts
GROUP BY author_display_name;
```
