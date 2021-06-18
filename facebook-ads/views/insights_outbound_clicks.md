---
title: Facebook Insights Outbound Clicks
description: Focused version of the insights outbound clicks data
usage: This is the "ready for analysis" view of Facebook Ads' insights outbound clicks information
modifications: -
---

```sql
-- CREATE VIEW fb_ads_insights_outbound_clicks AS
select
    oc.id as "outbound_click_id",
    oc.action_type as "action_type", -- will always be "outbound_click" in these tables. Used for joining to general action tables
    oc.facebook_ads_insights_id as "insights_id",
    oc."value" as "outbound_clicks_count",
    oc_ctr."value" as "outbound_clicks_ctr",
    cp_oc."value" as "cost_per_outbound_click",
    uoc."value" as "unique_outbound_clicks_count",
    uoc_ctr."value" as "unique_outbound_clicks_ctr",
    cp_uoc."value" as "cost_per_unique_outbound_click",
    oc."28d_click" as "outbound_28d_clicks_count",
    oc_ctr."28d_click" as "outbound_28d_clicks_ctr",
    cp_oc."28d_click" as "cost_per_outbound_28d_click",
    uoc."28d_click" as "unique_outbound_28d_clicks_count",
    uoc_ctr."28d_click" as "unique_outbound_28d_clicks_ctr",
    cp_uoc."28d_click" as "cost_per_unique_outbound_28d_click"
from
    facebook_ads_insights_outbound_clicks oc
    inner join facebook_ads_insights_outbound_clicks_ctr oc_ctr using (id)
    inner join facebook_ads_insights_cost_per_outbound_click cp_oc using (id)
    inner join facebook_ads_insights_unique_outbound_clicks uoc using (id)
    inner join facebook_ads_insights_unique_outbound_clicks_ctr uoc_ctr using (id)
    inner join facebook_ads_insights_cost_per_unique_outbound_click cp_uoc using (id);
```
