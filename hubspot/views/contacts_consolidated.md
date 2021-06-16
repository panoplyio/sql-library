---
title: Contacts consolidated
description: Denormalized view of Hubspot's contacts data. This view can be used together with other Hubspot views
usage: This is the "ready for analysis" view of Hubspot contacts
modifications: Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name. Dates are formatted as Unix timestamps so they must be transformed and cast into dates.
---

```sql
-- CREATE VIEW hubspot_contacts_consolidated AS
SELECT cp.*
	, hc.profile_url
	, hcm.merged_contact_id
	, hcm.merged_contact_first_name
	, hcm.merged_contact_last_name
	, hcm.merged_contact_properties_moved
FROM (
	SELECT id as contact_id
		, "profile-url" AS profile_url
	FROM hubspot_contacts
	WHERE case when "is-contact" = 1 then TRUE when "is-contact" = 0 then FALSE else "is-contact"::bigint::boolean end IS TRUE
	) hc
INNER JOIN (
	SELECT p.hubspot_contacts_id as contact_id
		-- Contact Info
		, MAX(CASE WHEN KEY = 'email' THEN value END) AS email -- Email (string)
		, MAX(CASE WHEN KEY = 'hs_email_domain' THEN value END) AS email_domain -- Email Domain (string)
		, MAX(CASE WHEN KEY = 'firstname' THEN value END) AS first_name -- First Name (string)
		, MAX(CASE WHEN KEY = 'lastname' THEN value END) AS last_name -- Last Name (string)
		, MAX(CASE WHEN KEY = 'twitterhandle' THEN value END) AS twitter_handle -- Twitter Username (string)
		, MAX(CASE WHEN KEY = 'city' THEN value END) AS city -- City (string)
		, MAX(CASE WHEN KEY = 'state' THEN value END) AS "state" -- State/Region (string)
		, MAX(CASE WHEN KEY = 'jobtitle' THEN value END) AS job_title -- Job Title (string)
		, MAX(CASE WHEN KEY = 'company' THEN value END) AS company -- Company Name (string)
		, MAX(CASE WHEN KEY = 'website' THEN value END) AS website -- Website URL (string)
		-- Dates and Durations
		, MAX(CASE WHEN KEY = 'createdate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS created_date -- Create Date (timestamp)
		, MAX(CASE WHEN KEY = 'lastmodifieddate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_modified_date -- Last Modified Date (timestamp)
		, MAX(CASE WHEN KEY = 'hs_lifecyclestage_lead_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS lifecyclestage_lead_date -- Became a Lead Date (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_first_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_timestamp -- Time First Seen (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_last_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_timestamp -- Time Last Seen (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_first_visit_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_visit_timestamp -- Time of First Session (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_last_visit_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_visit_timestamp -- Time of Last Session (timestamp)
		-- Sales Pipeline Info
		, MAX(CASE WHEN KEY = 'lifecyclestage' THEN value END) AS lifecycle_stage -- Lifecycle Stage (enumeration)
		, MAX(CASE WHEN KEY = 'hs_email_optout' THEN value END) AS email_optout -- Unsubscribed from all email (bool)
		, MAX(CASE WHEN KEY = 'hs_email_quarantined' THEN value END) AS email_quarantined -- Email Address Quarantined (bool)
		, MAX(CASE WHEN KEY = 'hs_all_contact_vids' THEN value END) AS all_contact_vids -- All vids for a contact (enumeration)
		-- Money
		, MAX(CASE WHEN KEY = 'hs_analytics_revenue' THEN value END) AS revenue -- Event Revenue (number)
		-- Numbers
		, MAX(CASE WHEN KEY = 'hs_analytics_average_page_views' THEN value END) AS average_page_views -- Average Pageviews (number)
		, MAX(CASE WHEN KEY = 'num_conversion_events' THEN value END) AS num_conversion_events -- Number of Form Submissions (number)
		, MAX(CASE WHEN KEY = 'num_unique_conversion_events' THEN value END) AS num_unique_conversion_events -- Number of Unique Forms Submitted (number)
        , MAX(CASE WHEN KEY = 'hs_analytics_num_page_views' THEN value END) AS num_page_views -- Number of Pageviews (number)
        , MAX(CASE WHEN KEY = 'hs_analytics_num_visits' THEN value END) AS num_visits -- Number of Sessions (number)
        , MAX(CASE WHEN KEY = 'hs_analytics_num_event_completions' THEN value END) AS num_event_completions -- Number of event completions (number)
		-- Data Source Fields
		, MAX(CASE WHEN KEY = 'hs_analytics_source' THEN value END) AS source -- Original Source (enumeration)
		, MAX(CASE WHEN KEY = 'hs_analytics_source_data_1' THEN value END) AS source_data_1 -- Original Source Drill-Down 1 (string)
		, MAX(CASE WHEN KEY = 'hs_analytics_source_data_2' THEN value END) AS source_data_2 -- Original Source Drill-Down 2 (string)
    FROM hubspot_contacts_properties p
	GROUP BY p.hubspot_contacts_id
	) cp ON cp.contact_id = hc.contact_id
LEFT JOIN (
	SELECT DISTINCT hcmv.hubspot_contacts_id AS merged_contact_id
		, "value" AS merged_vid
		, hcma."first-name" AS merged_contact_first_name
		, hcma."last-name" AS merged_contact_last_name
		, hcma."num-properties-moved" AS merged_contact_properties_moved
	FROM "hubspot_contacts_merged-vids" hcmv
	INNER JOIN "hubspot_contacts_merge-audits" hcma ON hcma.hubspot_contacts_id = hcmv.hubspot_contacts_id
	WHERE "value" <> hcmv.hubspot_contacts_id
	) hcm ON hcm.merged_contact_id = hc.contact_id;
```
