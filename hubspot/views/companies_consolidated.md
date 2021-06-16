---
title: Companies consolidated
description: Denormalized view of Hubspot's companies data. This view can be used together with other Hubspot views
usage: This is the "ready for analysis" view of Hubspot companies
modifications: Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name. Dates are formatted as Unix timestamps so they must be transformed and cast into dates.
---

```sql
-- CREATE VIEW hubspot_companies_consolidated AS
SELECT cp.*
FROM (
	SELECT companyid as company_id
	FROM hubspot_company
	WHERE isdeleted IS FALSE
	) hc
INNER JOIN (
	SELECT p.hubspot_company_id as company_id
		-- Company Details
		, MAX(CASE WHEN KEY = 'name' THEN value END) AS company_name -- Name (string)
		, MAX(CASE WHEN KEY = 'about_us' THEN value END) AS about_us -- About Us (string)
		, MAX(CASE WHEN KEY = 'description' THEN value END) AS description -- Description (string)
		, MAX(CASE WHEN KEY = 'domain' THEN value END) AS domain -- Company Domain Name (string)
		, MAX(CASE WHEN KEY = 'phone' THEN value END) AS phone -- Phone Number (string)
		, MAX(CASE WHEN KEY = 'address' THEN value END) AS address -- Street Address (string)
		, MAX(CASE WHEN KEY = 'address2' THEN value END) AS address2 -- Street Address 2 (string)
		, MAX(CASE WHEN KEY = 'city' THEN value END) AS city -- City (string)
		, MAX(CASE WHEN KEY = 'state' THEN value END) AS "state" -- State/Region (string)
		, MAX(CASE WHEN KEY = 'zip' THEN value END) AS zip -- Postal Code (string)
		, MAX(CASE WHEN KEY = 'country' THEN value END) AS country -- Country (string)
		, MAX(CASE WHEN KEY = 'timezone' THEN value END) AS timezone -- Time Zone (string)
		, MAX(CASE WHEN KEY = 'industry' THEN value END) AS industry -- Industry (enumeration)
		, MAX(CASE WHEN KEY = 'is_public' THEN value END) AS is_public -- Is Public (bool)
		, MAX(CASE WHEN KEY = 'linkedinbio' THEN value END) AS linkedin_bio -- LinkedIn Bio (string)
		, MAX(CASE WHEN KEY = 'linkedin_company_page' THEN value END) AS linkedin_company_page -- LinkedIn Company Page (string)
		, MAX(CASE WHEN KEY = 'facebook_company_page' THEN value END) AS facebook_company_page -- Facebook Company Page (string)
		, MAX(CASE WHEN KEY = 'twitterhandle' THEN value END) AS twitter_handle -- Twitter Handle (string)
		, MAX(CASE WHEN KEY = 'web_technologies' THEN value END) AS web_technologies -- Web Technologies (enumeration)
		, MAX(CASE WHEN KEY = 'website' THEN value END) AS website -- Website URL (string)
		, MAX(CASE WHEN KEY = 'founded_year' THEN value END) AS founded_year -- Year Founded (string)
		, MAX(CASE WHEN KEY = 'numberofemployees' THEN value END) AS number_of_employees -- Number of Employees (number)
		-- Dates and Durations
		, MAX(CASE WHEN KEY = 'createdate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS created_date -- Create Date (timestamp)
		, MAX(CASE WHEN KEY = 'closedate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS close_date -- Close Date (timestamp)
		, MAX(CASE WHEN KEY = 'days_to_close' THEN value END) AS days_to_close -- Days to Close (number)
		, MAX(CASE WHEN KEY = 'first_contact_createdate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_contact_date -- First Contact Create Date (timestamp)
		, MAX(CASE WHEN KEY = 'first_conversion_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_conversion_date -- First Conversion Date (timestamp)
		, MAX(CASE WHEN KEY = 'first_deal_created_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_deal_date -- First Deal Created Date (timestamp)
		, MAX(CASE WHEN KEY = 'notes_last_updated' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS notes_last_updated -- Last Activity Date (timestamp)
		, MAX(CASE WHEN KEY = 'notes_last_contacted' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS notes_last_contacted -- Last Contacted (timestamp)
		, MAX(CASE WHEN KEY = 'engagements_last_meeting_booked' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS engagements_last_meeting_booked -- Last Meeting Booked (timestamp)
		, MAX(CASE WHEN KEY = 'hs_lastmodifieddate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_modified_date -- Last Modified Date (timestamp)
		, MAX(CASE WHEN KEY = 'notes_next_activity_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS notes_next_activity_date -- Next Activity Date (timestamp)
		, MAX(CASE WHEN KEY = 'hubspot_owner_assigneddate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS owner_assigned_date -- Owner Assigned Date (timestamp)
		, MAX(CASE WHEN KEY = 'recent_conversion_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS recent_conversion_date -- Recent Conversion Date (timestamp)
		, MAX(CASE WHEN KEY = 'recent_deal_close_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS recent_deal_close_date -- Recent Deal Close Date (timestamp)
		, MAX(CASE WHEN KEY = 'hs_sales_email_last_replied' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS sales_email_last_replied -- Recent Sales Email Replied Date (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_first_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_timestamp -- Time First Seen (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_last_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_timestamp -- Time Last Seen (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_first_visit_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS first_visit_timestamp -- Time of First Session (timestamp)
		, MAX(CASE WHEN KEY = 'hs_analytics_last_visit_timestamp' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_visit_timestamp -- Time of Last Session (timestamp)
		-- Money
		, MAX(CASE WHEN KEY = 'annualrevenue' THEN value END) AS annual_revenue -- Annual Revenue (number)
		, MAX(CASE WHEN KEY = 'recent_deal_amount' THEN value END) AS recent_deal_amount -- Recent Deal Amount (number)
		, MAX(CASE WHEN KEY = 'total_money_raised' THEN value END) AS total_money_raised -- Total Money Raised (string)
		, MAX(CASE WHEN KEY = 'total_revenue' THEN value END) AS total_revenue -- Total Revenue (number)
		-- Data Source Fields
		, MAX(CASE WHEN KEY = 'hs_analytics_source' THEN value END) AS source -- Original Source Type (enumeration)
		, MAX(CASE WHEN KEY = 'hs_analytics_source_data_1' THEN value END) AS source_data_1 -- Original Source Data 1 (string)
		, MAX(CASE WHEN KEY = 'hs_analytics_source_data_2' THEN value END) AS source_data_2 -- Original Source Data 2 (string)
		-- Sales Pipeline Info
		, MAX(CASE WHEN KEY = 'hubspot_owner_id' THEN CAST(value as INT) END) AS owner_id -- Company owner (enumeration)
		, MAX(CASE WHEN KEY = 'hs_all_owner_ids' THEN value END) AS all_owner_ids -- All owner ids (enumeration)
		, MAX(CASE WHEN KEY = 'hs_lead_status' THEN value END) AS lead_status -- Lead Status (enumeration)
		, MAX(CASE WHEN KEY = 'lifecyclestage' THEN value END) AS lifecycle_stage -- Lifecycle Stage (enumeration)
		, MAX(CASE WHEN KEY = 'first_conversion_event_name' THEN value END) AS first_conversion_event -- First Conversion (string)
		, MAX(CASE WHEN KEY = 'recent_conversion_event_name' THEN value END) AS recent_conversion_event -- Recent Conversion (string)
		-- Numbers
		, MAX(CASE WHEN KEY = 'num_associated_contacts' THEN value END) AS num_associated_contacts -- Associated Contacts (number)
		, MAX(CASE WHEN KEY = 'num_associated_deals' THEN value END) AS num_associated_deals -- Associated Deals (number)
		, MAX(CASE WHEN KEY = 'num_conversion_events' THEN value END) AS num_conversion_events -- Number of Form Submissions (number)
		, MAX(CASE WHEN KEY = 'hs_analytics_num_page_views' THEN value END) AS num_page_views -- Number of Pageviews (number)
		, MAX(CASE WHEN KEY = 'num_notes' THEN value END) AS num_notes -- Number of Sales Activities (number)
		, MAX(CASE WHEN KEY = 'hs_analytics_num_visits' THEN value END) AS num_visits -- Number of Sessions (number)
		, MAX(CASE WHEN KEY = 'num_contacted_notes' THEN value END) AS num_contacted_notes -- Number of times contacted (number)
	FROM hubspot_company_properties p
	GROUP BY p.hubspot_company_id
	) cp ON cp.company_id = hc.company_id;
```
