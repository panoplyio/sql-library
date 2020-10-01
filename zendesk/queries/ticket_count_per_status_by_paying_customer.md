---
title: Zendesk & Salesforce: Ticket count by status per paying-existing customers for the Quarter-To-Date (QTD).
description: This query shows how many zendesk tickets per status a paying customer has. Paying customers are derived from Salesforce data and the ticket count is derived from the Zendesk data. User data from the two datasources are linked by email.
requirements: Collect the `Lead` and `Opportunity` objects with the Panoply Salesforce data source and also the `Users` and `Organizations` Resources form the Zendesk data source.
usage: This query can be displayed in a tabular or pivot form to display the ticket count per status.
modifications: The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source. The Date Range Filter using the `created_at` in the `WHERE` clause can be changed.
---

# Zendesk & Salesforce: Ticket count by status per paying-existing customers for the Quarter-To-Date (QTD).

```sql
WITH paying_users AS (
    SELECT
        DISTINCT sfl."email"
    FROM
        public.salesforce_lead sfl
    JOIN
        public.salesforce_opportunity sfo
      ON sfl."convertedopportunityid" = sfo."id"
    WHERE sfo.stagename = 'Closed Won'
),
users AS (
    SELECT
        zu."id",
        zu."name",
        zu."email",
        CASE
            WHEN zo."name" IS NULL THEN zu."email"
            ELSE zo."name"
        END AS requester
    FROM public.zendesk_users zu
    LEFT JOIN
        public.zendesk_organizations zo
      ON zu."organization_id" = zo."id"
    JOIN
        public.paying_users apu
      ON apzu."email" = zu."email"
)
select
    u."name",
    u."requester",
    COUNT(CASE WHEN zt."status" = 'new' THEN 1 END) AS "new",
    COUNT(CASE WHEN zt."status" = 'open' THEN 1 END) AS "open",
    COUNT(CASE WHEN zt."status" = 'pending' THEN 1 END) AS "pending",
    COUNT(CASE WHEN zt."status" = 'hold' THEN 1 END) AS "hold",
    COUNT(CASE WHEN zt."status" = 'solved' THEN 1 END) AS "solved",
    COUNT(CASE WHEN zt."status" = 'closed' THEN 1 END) AS "closed",
    COUNT(*) AS "total"
from
    zendesk_tickets zt
JOIN
    users u
  ON zt."submitter_id" = u."id"
WHERE
    EXTRACT(quarter FROM zt."created_at") = EXTRACT(quarter FROM CURRENT_DATE)
GROUP BY 1,2
ORDER BY 9 DESC
```

## Query Results Dictionary
Column | Description
---|---
`name`| Name of Zendesk User
`requester`| Zendesk Organization or the Zendesk User Email if organization is not set.
`new`| Zendesk tickets count under the "new" status
`open`| Zendesk tickets count under the "open" status
`pending`| Zendesk tickets count under the "pending" status
`hold`| Zendesk tickets count under the "hold" status
`solved`| Zendesk tickets count under the "solved" status
`closed`| Zendesk tickets count under the "closed" status
