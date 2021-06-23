---
title: Campaigns Metrics per Month
description: This query aggregates multiple metrics per month for all the campaigns.
usage: This query can be used to create line charts per metric or a high level overview of the Facebook Ads' campaigns per month.
---

```sql
select
    date_part(YEAR, c.start_time) AS "Ad Start Year",
    date_part(MONTH, c.start_time) AS "Ad Start Month",
    sum(i.clicks) as monthly_clicks,
    sum(i.unique_clicks) as monthly_unique_clicks,
    avg(i.clicks) as avg_clicks_per_ad,
    round(
        1.0 * sum(i.clicks) / nullif(sum(unique_clicks), 0),
        4
    ) as clicks_per_user,
    round(sum(i.cpc * i.clicks), 4) as total_cost,
    round(avg(i.cpc), 4) as avg_cost_per_click,
    round(avg(i.cpm), 4) as avg_cost_per_1000_impressions,
    round(avg(i.cpp), 4) as avg_cost_per_purchase,
    avg(i.ctr) as avg_ctr,
    avg(i.unique_ctr) as avg_unique_ctr
from
    fb_ads_campaigns c
    left join fb_ads_insights i on c.campaign_id = i.campaign_id
group by 1, 2
order by 1, 2;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `Ad Start Year`| Year of aggregation. |
| `Ad Start Month`| Month of aggregation. |
| `monthly_clicks`| Total monthly clicks. |
| `monthly_unique_clicks`| Total monthly unique clicks. |
| `avg_clicks_per_ad`| Average clicks per ad. |
| `clicks_per_user`| Average clicks per user. |
| `total_cost`| total cost. |
| `avg_cost_per_click`| Average cost per click per ad. |
| `avg_cost_per_1000_impressions`| Average cost per 1,000 impressions per ad. |
| `avg_cost_per_purchase`| Average cost per purchase (Facebook pixel) per ad. |
| `avg_ctr`| Average click-through rate. |
| `avg_unique_ctr`| Average unique click-through rate. |
