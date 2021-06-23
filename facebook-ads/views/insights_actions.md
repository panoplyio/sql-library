---
title: Facebook Insights Actions
description: Focused version of the insights actions data
usage: This is the "ready for analysis" view of Facebook Ads' insights actions information
modifications: -
---

```sql
-- CREATE VIEW fb_ads_insights_actions AS
select
    a.id as "action_id",
    a.facebook_ads_insights_id as "insights_id",
    a.action_type,
    a."value" as action_count,
    av."value" as action_value,
    cp_a."value" as "cost_per_action",
    ua."value" as "unique_action_count",
    cp_ua."value" as "cost_per_unique_action",

    wc.action_type as "website_ctr_action_type",
    wc."value" as "website_ctr",
    wpr.action_type as "website_purchase_roas_action_type",
    wpr."value" as "website_purchase_roas",

    a."1d_view" as "1d_view_count",
    av."1d_view" as "1d_view_value",
    cp_a."1d_view" as "cost_per_1d_view",
    ua."1d_view" as "unqiue_1d_view_count",
    cp_ua."1d_view" as "cost_per_unique_action_1d_view",
    wpr."1d_view" as "website_purchase_roas_1d_view",

    a."28d_click" as "28d_click_count",
    av."28d_click" as "28d_click_value",
    cp_a."28d_click" as "cost_per_28d_click",
    ua."28d_click" as "unique_28d_click_count",
    cp_ua."28d_click" as "cost_per_unique_action_28d_click",
    wpr."28d_click" as "website_purchase_roas_28d_click"
from
    facebook_ads_insights_actions a
    left join facebook_ads_insights_action_values av using (id)
    left join facebook_ads_insights_unique_actions ua using(id)
    left join facebook_ads_insights_cost_per_action_type cp_a using (id)
    left join facebook_ads_insights_cost_per_unique_action_type cp_ua using (id)
    left join facebook_ads_insights_website_ctr wc using (id)
    left join facebook_ads_insights_website_purchase_roas wpr using (id);
```
