---
title: Facebook Campaigns
description: Focused version of the campaigns data
usage: This is the "ready for analysis" view of Facebook Ads' campaigns information
modifications: Properties can be removed or added.
---

```sql
-- CREATE VIEW fb_ads_campaigns AS
SELECT
    c.id as "campaign_id",
    c.source_campaign_id,
    c.account_id,
    c.name,
    c.status,
    c.effective_status,
    c.special_ad_category,
    c.lifetime_budget,
    c.budget_remaining,
    c.daily_budget,
    c.last_budget_toggling_time,
    c.spend_cap,
    c.objective,
    c.bid_strategy,
    c.buying_type,
    cpt."value" as "pacing_type",
    c.can_use_spend_cap,
    c.can_create_brand_lift_study,
    c.start_time,
    c.stop_time,
    c.created_time,
    c.updated_time
FROM
    facebook_ads_campaigns c
    LEFT JOIN facebook_ads_campaigns_pacing_type cpt on c.id = cpt.facebook_ads_campaigns_id;
```
