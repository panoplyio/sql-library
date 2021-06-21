---
title: Target Audiences by Optimization Goal
description: This query counts the number of adsets targeting each audience per optimization goal.
usage: This query can be used to create an histogram of number of adsets in each audience per optimization goal.
modifications: Add or change target audiences.
---

```sql
select
	"as".optimization_goal
	, sum(case when t.target_gender = 'female' then 1 else null end) as female_targets
	, sum(case when t.target_device_platforms like 'mobile%' then 1 else null end) as mobile_device_targets
	, sum(case when t.target_device_platforms like 'desktop%' then 1 else null end) as desktop_targets
	, sum(case when t.publisher_platforms = 'instagram' then 1 else null end) as instagram_targets
	, sum(case when t.targeting_optimization = 'expansion_all' then 1 else null end) as expansion_all_target_optimization
from
	fb_ads_adsets "as"
	left join fb_ads_adsets_targeting t on "as".adset_id=t.adset_id
group by 1;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `optimization_goal`| Optimization goal of the adsets. |
| `female_targets`| Number of adsets that target females. |
| `mobile_device_targets`| Number of adsets that target mobile devices. |
| `desktop_targets`| Number of adsets that target desktop devices. |
| `instagram_targets`| Number of adsets that target Instagram users. |
| `expansion_all_target_optimization`| Number of adsets that uses targeting expansion. |
