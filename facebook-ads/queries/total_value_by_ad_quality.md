---
title: Total Value by Ad Quality
description: This query aggregates multiple metrics for each adset.
usage: This query can be used to see main KPIs for adsets. The KPIs include but not limited to quality informaion, impressions, reach, spend, etc.
modifications: -
---

```sql
select
    i.account_name,
    i.campaign_name,
    i.adset_name,
    sum(i.spend) as total_spend,
    sum(i.impressions) as total_impressions,
    sum(i.reach) as total_reach,
    sum(case when i.quality_ranking='BELOW_AVERAGE_10' then ia.action_value else null end) as quality_below_average_10_value
    ,sum(case when i.quality_ranking='BELOW_AVERAGE_20' then ia.action_value else null end) as quality_below_average_20_value
    ,sum(case when i.quality_ranking='BELOW_AVERAGE_35' then ia.action_value else null end) as quality_below_average_35_value
    ,sum(case when i.quality_ranking='AVERAGE' then ia.action_value else null end) as quality_below_average_value
    ,sum(case when i.quality_ranking='ABOVE_AVERAGE' then ia.action_value else null end) as quality_above_average_value
    ,sum(case when i.quality_ranking='UNKNOWN' then ia.action_value else null end) as quality_unknown_value
    ,sum(case when i.engagement_rate_ranking='BELOW_AVERAGE_10' then ia.action_value else null end) as engagement_rate_below_average_10_value
    ,sum(case when i.engagement_rate_ranking='BELOW_AVERAGE_20' then ia.action_value else null end) as engagement_rate_below_average_20_value
    ,sum(case when i.engagement_rate_ranking='BELOW_AVERAGE_35' then ia.action_value else null end) as engagement_rate_below_average_35_value
    ,sum(case when i.engagement_rate_ranking='AVERAGE' then ia.action_value else null end) as engagement_rate_below_average_value
    ,sum(case when i.engagement_rate_ranking='ABOVE_AVERAGE' then ia.action_value else null end) as engagement_rate_above_average_value
    ,sum(case when i.engagement_rate_ranking='UNKNOWN' then ia.action_value else null end) as engagement_rate_unknown_value
    ,sum(case when i.conversion_rate_ranking='BELOW_AVERAGE_10' then ia.action_value else null end) as conversion_rate_below_average_10_value
    ,sum(case when i.conversion_rate_ranking='BELOW_AVERAGE_20' then ia.action_value else null end) as conversion_rate_below_average_20_value
    ,sum(case when i.conversion_rate_ranking='BELOW_AVERAGE_35' then ia.action_value else null end) as conversion_rate_below_average_35_value
    ,sum(case when i.conversion_rate_ranking='AVERAGE' then ia.action_value else null end) as conversion_rate_below_average_value
    ,sum(case when i.conversion_rate_ranking='ABOVE_AVERAGE' then ia.action_value else null end) as conversion_rate_above_average_value
    ,sum(case when i.conversion_rate_ranking='UNKNOWN' then ia.action_value else null end) as conversion_rate_unknown_value
from
    fb_ads_insights i
    left join fb_ads_insights_actions ia on i.insights_id = ia.insights_id
    group by 1, 2, 3;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `account_name`| Ad account name. |
| `campaign_name`| The name of the campaign . |
| `adset_name`| The name of the adset. |
| `total_spend`| Total spend for adset. |
| `total_impressions`| Total impressions for adset. |
| `total_reach`| Total reach of adset. |
| `quality_below_average_10_value`| Total value for quality below average 10. |
| `quality_below_average_20_value`| Total value for quality below average 20. |
| `quality_below_average_35_value`| Total value for quality below average 35. |
| `quality_below_average_value`| Total value for quality below average. |
| `quality_above_average_value`| Total value for quality above average. |
| `quality_unknown_value`| Total value for quality unknown. |
| `engagement_rate_below_average_10_value`| Total value for engagement rate below average 10. |
| `engagement_rate_below_average_20_value`| Total value for engagement rate below average 20. |
| `engagement_rate_below_average_35_value`| Total value for engagement rate below average 35. |
| `engagement_rate_below_average_value`| Total value for engagement rate below average. |
| `engagement_rate_above_average_value`| Total value for engagement rate above average. |
| `engagement_rate_unknown_value`| Total value for engagement rate unknown. |
| `conversion_rate_below_average_10_value`| Total value for conversion rate below average 10. |
| `conversion_rate_below_average_20_value`| Total value for conversion rate below average 20. |
| `conversion_rate_below_average_35_value`| Total value for conversion rate below average 35. |
| `conversion_rate_below_average_value`| Total value for conversion rate below average. |
| `conversion_rate_above_average_value`| Total value for conversion rate above average. |
| `conversion_rate_unknown_value`| Total value for conversion rate unknown. |
