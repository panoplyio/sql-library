---
title: Hubspot - Time elapsed histogram: From contact create date to MQL conversion
description: This query quantifies how long it takes for Hubspot contacts to become Marketing Qualified Leads. Contacts are grouped into bins based on the elapsed time from contact to MQL. ('Day 1', 'Day 2-7', 'Day 8-28' and 'Days 28+'). This data is pulled from the [Hubspot API into Panoply](https://panoply.io/docs/data-sources/hubspot/).
usage: This query can be used to create histogram of time elapsed between the creation of a contact and their conversion to a marketing qualified lead. You can follow the [full tutorial here](https://blog.panoply.io/hubspot-lead-analytics-in-sql-beyond-crm-reporting).
modifications: Change the bin size by altering BETWEEN integers (Ex. BETWEEN 1 AND 6) and the associated column names (in single quotes after THEN).
---

```sql
SELECT
  CASE WHEN DATEDIFF(
    'day', "createdate", "hs_lifecyclestage_marketingqualifiedlead_date") = 0
    THEN 'Day 1'
  WHEN DATEDIFF(
    'day', "createdate", "hs_lifecyclestage_marketingqualifiedlead_date")
    BETWEEN 1 AND 6 THEN 'Day 2-7'
  WHEN DATEDIFF(
    'day', "createdate", "hs_lifecyclestage_marketingqualifiedlead_date")
    BETWEEN 6 AND 27 THEN 'Day 8-28'
  ELSE 'Days 28+' END AS bins_days_diff,
  COUNT(distinct hubspot_contacts_id)
FROM
  (
    SELECT
      p.hubspot_contacts_id,
      MIN(
        CASE WHEN "key" = 'createdate' THEN timestamp 'epoch' + p.value / 1000
        * interval '1 second' ELSE NULL END
      ) createdate,
      MIN(
        CASE WHEN "key" = 'hs_lifecyclestage_marketingqualifiedlead_date' THEN
        timestamp 'epoch' + p.value / 1000 * interval '1 second' ELSE NULL END
      ) hs_lifecyclestage_marketingqualifiedlead_date
    FROM
      hubspot_contacts_properties p
    GROUP BY 1
  ) contact_dates
WHERE
  createdate IS NOT NULL
  AND hs_lifecyclestage_marketingqualifiedlead_date IS NOT NULL
  AND bins_days_diff IS NOT NULL
GROUP by bins_days_diff
ORDER BY bins_days_diff
```

## Query Results Dictionary
Column | Description
---|---
`bins_days_diff`| Bin indicating the elapsed between contact creation and MQL conversion date.
`count`| The number of Hubspot contacts that fall into the bins_days_diff groupings.
