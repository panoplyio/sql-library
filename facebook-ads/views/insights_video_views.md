---
title: Facebook Insights Video Views
description: Focused version of the insights video views data
usage: This is the "ready for analysis" view of Facebook Ads' insights video views information
---

```sql
-- CREATE VIEW fb_ads_insights_video_views AS
select
    nvl(
        atwa.id,
        wa_30s.id,
        p25.id,
        p50.id,
        p75.id,
        p95.id,
        p100.id
    ) as "video_action_id",
    nvl(
        atwa.facebook_ads_insights_id,
        wa_30s.facebook_ads_insights_id,
        p25.facebook_ads_insights_id,
        p50.facebook_ads_insights_id,
        p75.facebook_ads_insights_id,
        p95.facebook_ads_insights_id,
        p100.facebook_ads_insights_id
    ) as "insights_id",
    nvl(
        atwa.action_type,
        wa_30s.action_type,
        p25.action_type,
        p50.action_type,
        p75.action_type,
        p95.action_type,
        p100.action_type
    ) as "action_type",
    atwa."value" as "avg_time_watched",
    atwa."1d_view" as "avg_time_watched_1d_view",
    atwa."28d_click" as "avg_time_watched_28d_click",
    wa_30s."value" as "30_seconds_watched_count",
    wa_30s."1d_view" as "30_seconds_watched_count_1d_view",
    wa_30s."28d_click" as "30_seconds_watched_count_28d_click",
    p25."value" as "25_percent_watched_count",
    p25."1d_view" as "25_percent_watched_count_1d_view",
    p25."28d_click" as "25_percent_watched_count_28d_click",
    p50."value" as "50_percent_watched_count",
    p50."1d_view" as "50_percent_watched_count_1d_view",
    p50."28d_click" as "50_percent_watched_count_28d_click",
    p75."value" as "75_percent_watched_count",
    p75."1d_view" as "75_percent_watched_count_1d_view",
    p75."28d_click" as "75_percent_watched_count_28d_click",
    p95."value" as "95_percent_watched_count",
    p95."1d_view" as "95_percent_watched_count_1d_view",
    p95."28d_click" as "95_percent_watched_count_28d_click",
    p100."value" as "100_percent_watched_count",
    p100."1d_view" as "100_percent_watched_count_1d_view",
    p100."28d_click" as "100_percent_watched_count_28d_click"
from
    facebook_ads_insights_video_avg_time_watched_actions atwa
    full join facebook_ads_insights_video_30_sec_watched_actions wa_30s using (id)
    full join facebook_ads_insights_video_p25_watched_actions p25 using (id)
    full join facebook_ads_insights_video_p50_watched_actions p50 using (id)
    full join facebook_ads_insights_video_p75_watched_actions p75 using (id)
    full join facebook_ads_insights_video_p95_watched_actions p95 using (id)
    full join facebook_ads_insights_video_p100_watched_actions p100 using (id);
```
