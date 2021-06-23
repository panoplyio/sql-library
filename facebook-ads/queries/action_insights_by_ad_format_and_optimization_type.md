---
title: Action Insights by Ad Format and Optimization Type
description: This query aggregates actions metrics per the format and optimization type of each ad
usage: This query can be used to create an histogram of multiple metrics for ads' formats and optimization types
---

```sql
SELECT
    ac."status",
    ac.ad_format,
    ac.optimization_type,
    sum(ia.action_count) AS total_actions,
    sum(ia.action_value) AS total_value_of_actions,
    sum(ia.cost_per_action) AS total_cost_of_actions,
    avg(ia.action_count) AS avg_action_count,
    avg(ia.action_value) AS avg_value_per_action,
    avg(ia.cost_per_action) AS avg_cost_per_action,
    round(
        1.0 * sum(iv.avg_time_watched) / nullif(count(iv.video_action_id), 0),
        2
    ) as avg_time_watched_per_ad_with_video,
    round(
        1.0 * sum(iv."100_percent_watched_count") / nullif(count(i.insights_id), 0),
        2
    ) as fully_watched_video_percent
FROM
    fb_ads_ads_adcreatives ac
    LEFT JOIN fb_ads_insights i ON i.ad_id = ac.ad_id
    LEFT JOIN fb_ads_insights_actions ia ON ia.insights_id = i.insights_id
    LEFT JOIN fb_ads_insights_video_views iv on i.insights_id = iv.insights_id
GROUP BY 1, 2, 3;
```

## Query Results Dictionary

| Column | Description |
| --- | --- |
| `status`| The ad creative status |
| `ad_format`| The ad format |
| `optimization_type`| The ad's optimization type |
| `total_actions`| Total number of actions |
| `total_value_of_actions`| Total value of all actions |
| `total_cost_of_actions`| Total cost of all actions |
| `avg_action_count`| Average number of actions |
| `avg_value_per_action`| Average value per action |
| `avg_cost_per_action`| Average cost per action |
