---
title: Salesforce - Active Sales Rep lead-to-opportunity Conversion and Closed/Won Opportunities Rate for the Quarter-To-Date (QTD)
description: This query shows the conversion count and rate of lead-to-opportunity conversion and closed opportunities per active sales rep derived from Salesforce data.
requirements: Collect the `User`, `Opportunity`, and `Lead` objects with the Panoply Salesforce data source
usage: This query can be displayed in a tabular form to display the count and rate per active sales rep
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Date Range Filter using the `createddate` in the `WHERE` clause can be changed.
---

# Salesforce - Active Sales Rep lead-to-opportunity Conversion and Closed/Won Opportunities Rate for the Quarter-To-Date (QTD)

```sql
WITH active_user AS (
  SELECT
    id, email, firstname || ' ' || lastname AS rep
  FROM
    public.salesforce_user
  WHERE
    salesforce_user.IsActive = 1
),
converted_and_won AS (
  SELECT
    sl.ownerid,
    COUNT(*) AS "total_leads",
    COUNT(CASE WHEN sl.convertedopportunityid IS NOT NULL THEN 1 END) AS "converted_leads",
    COUNT(CASE WHEN so.stagename = 'Closed Won' THEN 1 END) AS "won_leads",
    ROUND(converted_leads * 100.0 / total_leads, 2) AS "conversion_rate",
    ROUND(won_leads * 100.0 / total_leads, 2) AS "win_rate"
  FROM
    public.salesforce_lead sl
  LEFT JOIN
    public.salesforce_opportunity so
    ON sl.convertedopportunityid = so.id
  WHERE
    DATE_TRUNC('quarter', sl."createddate") = DATE_TRUNC('quarter', CURRENT_DATE)
  GROUP BY
    1
)
SELECT
  au.rep AS "sales_rep",
  au.email,
  srl.total_leads,
  srl.converted_leads,
  srl.won_leads,
  srl.conversion_rate,
  srl.win_rate
FROM
  converted_and_won srl
JOIN
  active_user au
  ON srl.ownerid = au.id
ORDER BY
  total_leads desc
```

## Query Results Dictionary
Column | Description
---|---
`sales_rep`| Opportunity Owner (Sales Rep)
`email`| Sales Rep Email
`total_leads`| Total Leads Owned by the Sales Rep
`converted_leads`| Total Leads Converted to Opportunity by the Sales Rep
`won_leads`| Total Opportunities moved to the Closed/Won stage
`conversion_rate`| Rate of conversion from Lead to Opportunity
`win_rate`| Rate of opportunities being closed/won
