---
title: Facebook Adsests
description: Focused version of the adsets data
usage: This is the "ready for analysis" view of Facebook Ads' adsets information
modifications: Properties can be removed or added.
---

```sql
-- CREATE VIEW fb_ads_adsets AS
SELECT
    id as "adset_id",
    source_adset_id,
    account_id,
    campaign_id,
    name,
    status,
    effective_status,
    lifetime_budget,
    budget_remaining,
    daily_budget,
    daily_min_spend_target,
    recurring_budget_semantics,
    bid_strategy,
    optimization_goal,
    optimization_sub_event,
    destination_type,
    billing_event,
    lifetime_imps,
    use_new_app_click,
    multi_optimization_goal_weight,
    is_dynamic_creative,
    start_time,
    end_time,
    updated_time,
    created_time
FROM
    facebook_ads_adsets;
```
