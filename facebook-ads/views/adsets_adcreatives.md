---
title: Facebook Adsets Ad Creatives
description: Focused version of the adsets ad creatives data
usage: This is the "ready for analysis" view of Facebook Ads' adsets ad creatives information
modifications: Properties can be removed or added.
---

```sql
-- CREATE VIEW fb_ads_adsets_adcreatives AS
SELECT aac.id AS "adsets_adcreatives_id"
	, aac.facebook_ads_adsets_id AS "adset_id"
	, aac.account_id
	, aac.name
	, CASE WHEN aac.title IS NULL THEN afs_t.TEXT ELSE aac.title END AS "title"
	, CASE WHEN aac."body" IS NULL THEN afs_b.TEXT ELSE aac.body END AS "text_body"
	, afs_d."text" AS "description"
	, aac.STATUS
	, aac.object_type
	, CASE WHEN aac.call_to_action_type IS NULL THEN afs_cta."value" ELSE aac.call_to_action_type END AS "call_to_action_type"
	, aac.authorization_category
	, aac.effective_authorization_category
	, afs.optimization_type
	, afs_af."value" AS "ad_format"
	, afs_lu.website_url AS "website_url"
	, aac.instagram_permalink_url
	, aac.thumbnail_url
	, aac.image_url
	, aac.image_hash
	, aac.url_tags
FROM facebook_ads_adsets_adcreatives aac
LEFT JOIN facebook_ads_adsets_adcreatives_asset_feed_spec afs ON aac.id = afs.facebook_ads_adsets_adcreatives_id
LEFT JOIN facebook_ads_adsets_adcreatives_asset_feed_spec_ad_formats afs_af ON afs.id = afs_af.facebook_ads_adsets_adcreatives_asset_feed_spec_id
LEFT JOIN facebook_ads_adsets_adcreatives_asset_feed_spec_titles afs_t ON afs.id = afs_t.facebook_ads_adsets_adcreatives_asset_feed_spec_id
LEFT JOIN facebook_ads_adsets_adcreatives_asset_feed_spec_bodies afs_b ON afs.id = afs_b.facebook_ads_adsets_adcreatives_asset_feed_spec_id
LEFT JOIN facebook_ads_adsets_adcreatives_asset_feed_spec_descriptions afs_d ON afs.id = afs_d.facebook_ads_adsets_adcreatives_asset_feed_spec_id AND afs_d."text" IS NOT NULL
LEFT JOIN facebook_ads_adsets_adcreatives_asset_feed_spec_call_to_action_types afs_cta ON afs.id = afs_cta.facebook_ads_adsets_adcreatives_asset_feed_spec_id
LEFT JOIN (
	SELECT DISTINCT facebook_ads_adsets_adcreatives_asset_feed_spec_id
		, website_url
	FROM facebook_ads_adsets_adcreatives_asset_feed_spec_link_urls
) afs_lu ON afs.id = afs_lu.facebook_ads_adsets_adcreatives_asset_feed_spec_id;
```
