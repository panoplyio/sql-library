
Instructions | Details
---
title: Salesforce - Account to Email
description: View for getting the connection between Salesforce `accountid` to the relevant user `email`.
requirements: Collect the `Account`, `Contact` and `Lead` objects with the Panoply Salesforce data source.
usage: This view can be used in many different salesforce reports for a quick connection between `accountid` and the relevant `email`.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. Different filters can be added throughout the query, either in the subquery or by adding a `WHERE` clause in the final query.
---

# Salesforce - Account to Email

```sql
-- CREATE VIEW salesforce_account_to_email AS
WITH email_accountid AS (
  SELECT
    DISTINCT sfc.email,
    sfc.accountid
  FROM
    public.salesforce_contact sfc
  UNION -- Leads and Contacts are combined by UNION because they are mutually exclusive groups of individuals
  SELECT
    DISTINCT sfl.email,
    sfl.convertedaccountid
  FROM
    public.salesforce_lead sfl
)
SELECT
  DISTINCT eaid.email,
  eaid.accountid,
  sfa.name account_name
FROM
  email_accountid eaid
  LEFT JOIN public.salesforce_account sfa ON eaid.accountid = sfa.id
```

## View Results Dictionary
Column | Description
---|---
`email`| Opportunity Email
`accountid`| Salesforce Account ID
`account_name`| Account Name

## Example: New Opportunities Summary
[See Query Details Here](https://github.com/panoplyio/sql-library/blob/master/salesforce/queries/new_opps_summary.md)
