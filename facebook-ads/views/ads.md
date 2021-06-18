---
title: Facebook Ads
description: Focused version of the ads data
usage: This is the "ready for analysis" view of Facebook Ads' ads information
modifications: Properties can be removed or added.
---

```sql
-- CREATE VIEW fb_ads_ads AS
SELECT id AS "ad_id"
	, source_ad_id
	, account_id
	, campaign_id
	, adset_id
	, "name"
	, preview_shareable_link
	, "STATUS"
	, effective_status
	, last_updated_by_app_id
	, created_time
	, updated_time
FROM facebook_ads_ads;
```
