---
title: Facebook Insights
description: Focused version of the insights data
usage: This is the "ready for analysis" view of Facebook Ads' insights information
modifications: Properties can be removed or added.
---

```sql
-- CREATE VIEW fb_ads_insights AS
SELECT
    id as "insights_id",
    account_id,
    campaign_id,
    adset_id,
    ad_id,
    account_name,
    campaign_name,
    adset_name,
    ad_name,
    objective,
    spend,
    social_spend,
    account_currency,
    buying_type,
    frequency,
    reach,
    impressions,
    quality_ranking,
    engagement_rate_ranking,
    conversion_rate_ranking,
    clicks,
    cpc,
    cpm,
    cpp,
    ctr,
    unique_clicks,
    cost_per_unique_click,
    unique_ctr,
    unique_link_clicks_ctr,
    inline_link_clicks,
    cost_per_inline_link_click,
    inline_link_click_ctr,
    unique_inline_link_clicks,
    cost_per_unique_inline_link_click,
    unique_inline_link_click_ctr,
    inline_post_engagement,
    cost_per_inline_post_engagement,
    date_start,
    date_stop
FROM
    facebook_ads_insights;
```
