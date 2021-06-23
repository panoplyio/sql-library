---
title: Facebook Adsets Targeting Custom Audiences
description: Focused version of the adsets targeting data for custom audiences only
usage: This is the "ready for analysis" view of Facebook Ads' adsets targeting for custom audiences information
---

```sql
-- CREATE VIEW fb_adsets_targeting_custom_audiences AS
select
    ca.id || '_ca' as "custom_audience_id",
    ca.facebook_ads_adsets_targeting_id as "targeting_id",
    ca."name" as "audience_name",
    null as "excluded"
from
    facebook_ads_adsets_targeting_custom_audiences ca
union
select
    eca.id || '_eca' as "custom_audience_id",
    eca.facebook_ads_adsets_targeting_id as "targeting_id",
    eca."name" as "audience_name",
    True as "excluded"
from
    facebook_ads_adsets_targeting_excluded_custom_audiences eca;
```
