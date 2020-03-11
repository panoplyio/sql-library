# ﻿Hubspot Form Submission Analysis

Instructions | Details
---|---
Description | This query calculates metrics about form submissions and the context of the form submissions. This data is pulled from the [Hubspot API into Panoply](https://panoply.io/docs/data-sources/hubspot/).
Usage | This query can be used to create a bar chart that identifies the most commonly submitted Hubspot forms. Further modification could allow you to analyze the form submission over time, or the sequence of form submissions for each contact.
Modifications | Remove the dimensions of the query to aggregate the metrics at a higher level. For instance, if one form can be submitted on multiple pages, removing the `page_url` and `page_title` dimensions would provide _form-level_ metrics only. Modify the `submission_timestamp` comparison in the `WHERE` clause to change the time frame of the analysis.

```sql
WITH form_submissions
AS (
  SELECT s."conversion-id" submission_id,
    TIMESTAMP 'epoch' + s.TIMESTAMP / 1000 * interval '1 second' submission_timestamp,
    row_number() OVER (
      PARTITION BY s.hubspot_contacts_id ORDER BY s.TIMESTAMP
      ) submission_order,
    s."page-id" page_id,
    s."page-url" page_url,
    s."page-title" page_title,
    s.title form_title,
    s."form-id" form_id,
    s.hubspot_contacts_id
  FROM "hubspot_contacts_form-submissions" s
  )
SELECT f.guid form_id,
  f.name form_current_name,
  fs.page_url,
  fs.page_title,
  fs.form_title,
  count(DISTINCT fs.submission_id) total_submissions,
  count(DISTINCT fs.hubspot_contacts_id) total_contacts,
  count(DISTINCT CASE WHEN submission_order = 1 THEN fs.submission_id END) first_submissions,
  round(total_submissions::float / total_contacts, 2) submission_per_contact
FROM hubspot_forms f
JOIN form_submissions fs ON fs.form_id = f.guid
WHERE submission_timestamp > CURRENT_DATE - 28
GROUP BY 1,2,3,4,5;
```

## Query Results Dictionary
Column | Description
---|---
`form_id`| Hubspot's internal canonical form identifier, sometimes referred to as the `guid`
`form_current_name`| Most recent name of the form in the Hubspot app (form names are mutable)
`page_url`| The full URL the form was submitted on excluding protocol
`page_title`| The title of the page where the form was submitted at the time of submission
`form_title`| The name of the form at the time of submission
`total_submissions`| Count of unique form submissions for each form, also referred to as "conversions"
`total_contacts`| Count of unique contacts who submitted each form
`first_submissions`| Count of times that this was a contacts' first form submission (often point of acquisition)
`submission_per_contact`| Average number of submission per contact
