---
title: Facebook Adsets Targeting
description: Focused version of the adsets targeting data
usage: This is the "ready for analysis" view of Facebook Ads' adsets targeting information
modifications: -
---

```sql
-- CREATE VIEW fb_ads_adsets_targeting AS
select
    t.id as "targeting_id",
    t.facebook_ads_adsets_id as "adset_id",
    ex.id as "exclusions_id",
    geo.id as "geo_locations_id",
    t.age_max,
    t.age_min,
    t.targeting_optimization,
    case
        tg."value"
        when 2 then 'female'
        when 1 then 'male'
        else null
    end as "target_gender",
    pp."value" as "publisher_platforms",
    igp.instagram_positions as "instagram_positions",
    dv.device as "target_device_platforms"
from
    facebook_ads_adsets_targeting t
    left join facebook_ads_adsets_targeting_exclusions ex on t.id = ex.facebook_ads_adsets_targeting_id
    left join facebook_ads_adsets_targeting_genders tg on t.id = tg.facebook_ads_adsets_targeting_id
    left join facebook_ads_adsets_targeting_geo_locations geo on t.id = geo.facebook_ads_adsets_targeting_id
    left join facebook_ads_adsets_targeting_publisher_platforms pp on t.id = pp.facebook_ads_adsets_targeting_id
    left join (
        select
            facebook_ads_adsets_targeting_id,
            listagg(value, '; ') within group (
                ORDER BY
                    value
            ) as instagram_positions
        from
            facebook_ads_adsets_targeting_instagram_positions
        group by
            1
    ) igp on t.id = igp.facebook_ads_adsets_targeting_id
    left join (
        select
            facebook_ads_adsets_targeting_id,
            listagg (value, '; ') within group (
                order by
                    value
            ) as device
        from
            facebook_ads_adsets_targeting_device_platforms
        group by
            1
    ) dv on t.id = dv.facebook_ads_adsets_targeting_id;
```
