---
title: Facebook Ad Accounts
description: Focused version of the ad accounts data
usage: This is the "ready for analysis" view of Facebook Ads' ad account information
modifications: Properties can be removed or added.
---

```sql
-- CREATE VIEW fb_ads_ad_account AS
select
    account_id,
    name,
    account_status,
    is_personal,
    age,
    business_country_code,
    fb_entity,
    spend_cap,
    balance,
    amount_spent,
    currency,
    funding_source,
    min_daily_budget,
    min_campaign_group_spend_cap,
    is_prepay_account,
    timezone_id,
    timezone_name,
    timezone_offset_hours_utc,
    tax_id_status,
    is_tax_id_required,
    has_migrated_permissions,
    is_in_middle_of_local_entity_migration,
    is_notifications_enabled,
    can_create_brand_lift_study,
    offsite_pixels_tos_accepted,
    is_attribution_spec_system_default,
    is_in_3ds_authorization_enabled_market,
    is_direct_deals_enabled,
    created_time
from
    facebook_ads_ad_accounts;
```
