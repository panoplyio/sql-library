---
title: Salesforce - New Opportunities Summary
description: This query shows different details about opportunities derived from Salesforce data.
requirements: Collect the `Opportunity`, `User`, `Account`, `Contact` and `Lead` objects with the Panoply Salesforce data source and create the view `salesforce_account_to_email`, more details [here](https://github.com/panoplyio/sql-library/blob/master/salesforce/views/account_to_email.md).
usage: This query can be displayed in a tabular form to display the current opportunities.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Filters for `closedate, stagename, probability` in the `WHERE` clause can be changed or completely removed.
---

# Salesforce - New Opportunities Summary

```sql
SELECT
  sfu.name opp_owner,
  sfo.stagename stage,
  sfo.probability,
  sfa.name opp_name,
  sfae.email opp_email,
  sfo.amount,
  sfo.nextstep next_step,
  sfo.closedate close_date,
  sfo.createddate created_date
FROM
  public.salesforce_opportunity sfo
  JOIN public.salesforce_user sfu ON sfo.ownerid = sfu.id
  LEFT JOIN public.salesforce_account sfa on sfa.id = sfo.accountid
  LEFT JOIN public.salesforce_account_to_email sfae ON sfa.id = sfae.accountid
WHERE
  sfo.closedate between '2020-01-01' AND '2020-03-31' -- Dates can be changed \ Filter can be removed
  AND sfo.stagename in ('Identified Need', 'Trial', 'Contract Sent', 'Verbal Commit') -- Stage Names can be changed \ Filter can be removed
  AND sfo.probability > 10 -- Value can be changed \ Filter can be removed
ORDER BY
  1,
  2,
  3 DESC
```

## Query Results Dictionary
Column | Description
---|---
`opp_owner`| Opportunity Owner (Sales Rep)
`stage`| Opportunity Stage
`probability`| Opportunity close probability
`opp_name`| Opportunity Name
`opp_email`| Opportunity Email
`amount`| Deal Amount
`next_step`| Next Step
`close_date`| Close Date
`created_date`| Created Date
