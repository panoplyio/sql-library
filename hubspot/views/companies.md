---
title: Hubspot - Companies Report
description: Denormalized view of Hubspot's default values in `hubspot_companies_properties` key-value pair table
usage: This is the "ready for analysis" view of Hubspot companies
modifications: Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name. Dates are formatted as Unix timestamps so they must be transformed and cast into dates.
---

```sql
-- CREATE VIEW hubspot_companies_report AS
SELECT 	
  -- COMPANY DETAILS --
	p.hubspot_company_id,
    max( CASE WHEN key = 'about_us' THEN value END ) AS about_us, -- About Us (string)
    max( CASE WHEN key = 'hs_all_accessible_team_ids' THEN value END ) AS hs_all_accessible_team_ids, -- All accessible team ids (enumeration)
    max( CASE WHEN key = 'hs_all_owner_ids' THEN value END ) AS hs_all_owner_ids, -- All owner ids (enumeration)
    max( CASE WHEN key = 'hs_all_team_ids' THEN value END ) AS hs_all_team_ids, -- All team ids (enumeration)
    max( CASE WHEN key = 'annualrevenue' THEN value END ) AS annualrevenue, -- Annual Revenue (number)
    max( CASE WHEN key = 'num_associated_contacts' THEN value END ) AS num_associated_contacts, -- Associated Contacts (number)
    max( CASE WHEN key = 'num_associated_deals' THEN value END ) AS num_associated_deals, -- Associated Deals (number)
    max( CASE WHEN key = 'city' THEN value END ) AS city, -- City (string)
    max( CASE WHEN key = 'closedate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS closedate, -- Close Date (timestamp)
    max( CASE WHEN key = 'domain' THEN value END ) AS domain, -- Company Domain Name (string)
    max( CASE WHEN key = 'hs_object_id' THEN value END ) AS hs_object_id, -- Company ID (number)
    max( CASE WHEN key = 'hubspot_owner_id' THEN value END ) AS hubspot_owner_id, -- Company owner (enumeration)
    max( CASE WHEN key = 'country' THEN value END ) AS country, -- Country (string)
    max( CASE WHEN key = 'createdate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS createdate, -- Create Date (timestamp)
    max( CASE WHEN key = 'days_to_close' THEN value END ) AS days_to_close, -- Days to Close (number)
    max( CASE WHEN key = 'description' THEN value END ) AS description, -- Description (string)
    max( CASE WHEN key = 'facebook_company_page' THEN value END ) AS facebook_company_page, -- Facebook Company Page (string)
    max( CASE WHEN key = 'first_contact_createdate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS first_contact_createdate, -- First Contact Create Date (timestamp)
    max( CASE WHEN key = 'first_conversion_event_name' THEN value END ) AS first_conversion_event_name, -- First Conversion (string)
    max( CASE WHEN key = 'first_conversion_date' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS first_conversion_date, -- First Conversion Date (timestamp)
    max( CASE WHEN key = 'first_deal_created_date' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS first_deal_created_date, -- First Deal Created Date (timestamp)
    max( CASE WHEN key = 'hs_analytics_first_touch_converting_campaign' THEN value END ) AS hs_analytics_first_touch_converting_campaign, -- First Touch Converting Campaign (string)
    max( CASE WHEN key = 'hubspot_team_id' THEN value END ) AS hubspot_team_id, -- HubSpot Team (enumeration)
    max( CASE WHEN key = 'industry' THEN value END ) AS industry, -- Industry (enumeration)
    max( CASE WHEN key = 'is_public' THEN value END ) AS is_public, -- Is Public (bool)
    max( CASE WHEN key = 'notes_last_updated' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS notes_last_updated, -- Last Activity Date (timestamp)
    max( CASE WHEN key = 'notes_last_contacted' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS notes_last_contacted, -- Last Contacted (timestamp)
    max( CASE WHEN key = 'engagements_last_meeting_booked' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS engagements_last_meeting_booked, -- Last Meeting Booked (timestamp)
    max( CASE WHEN key = 'hs_lastmodifieddate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_lastmodifieddate, -- Last Modified Date (timestamp)
    max( CASE WHEN key = 'hs_analytics_last_touch_converting_campaign' THEN value END ) AS hs_analytics_last_touch_converting_campaign, -- Last Touch Converting Campaign (string)
    max( CASE WHEN key = 'hs_lead_status' THEN value END ) AS hs_lead_status, -- Lead Status (enumeration)
    max( CASE WHEN key = 'lifecyclestage' THEN value END ) AS lifecyclestage, -- Lifecycle Stage (enumeration)
    max( CASE WHEN key = 'hs_predictivecontactscore_v2' THEN value END ) AS hs_predictivecontactscore_v2, -- Likelihood to close (number)
    max( CASE WHEN key = 'linkedinbio' THEN value END ) AS linkedinbio, -- LinkedIn Bio (string)
    max( CASE WHEN key = 'linkedin_company_page' THEN value END ) AS linkedin_company_page, -- LinkedIn Company Page (string)
    max( CASE WHEN key = 'name' THEN value END ) AS name, -- Name (string)
    max( CASE WHEN key = 'notes_next_activity_date' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS notes_next_activity_date, -- Next Activity Date (timestamp)
    max( CASE WHEN key = 'numberofemployees' THEN value END ) AS numberofemployees, -- Number of Employees (number)
    max( CASE WHEN key = 'num_conversion_events' THEN value END ) AS num_conversion_events, -- Number of Form Submissions (number)
    max( CASE WHEN key = 'hs_analytics_num_page_views' THEN value END ) AS hs_analytics_num_page_views, -- Number of Pageviews (number)
    max( CASE WHEN key = 'num_notes' THEN value END ) AS num_notes, -- Number of Sales Activities (number)
    max( CASE WHEN key = 'hs_analytics_num_visits' THEN value END ) AS hs_analytics_num_visits, -- Number of Sessions (number)
    max( CASE WHEN key = 'num_contacted_notes' THEN value END ) AS num_contacted_notes, -- Number of times contacted (number)
    max( CASE WHEN key = 'hs_analytics_source_data_1' THEN value END ) AS hs_analytics_source_data_1, -- Original Source Data 1 (string)
    max( CASE WHEN key = 'hs_analytics_source_data_2' THEN value END ) AS hs_analytics_source_data_2, -- Original Source Data 2 (string)
    max( CASE WHEN key = 'hs_analytics_source' THEN value END ) AS hs_analytics_source, -- Original Source Type (enumeration)
    max( CASE WHEN key = 'hubspot_owner_assigneddate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hubspot_owner_assigneddate, -- Owner Assigned Date (timestamp)
    max( CASE WHEN key = 'phone' THEN value END ) AS phone, -- Phone Number (string)
    max( CASE WHEN key = 'zip' THEN value END ) AS zip, -- Postal Code (string)
    max( CASE WHEN key = 'recent_conversion_event_name' THEN value END ) AS recent_conversion_event_name, -- Recent Conversion (string)
    max( CASE WHEN key = 'recent_conversion_date' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS recent_conversion_date, -- Recent Conversion Date (timestamp)
    max( CASE WHEN key = 'recent_deal_amount' THEN value END ) AS recent_deal_amount, -- Recent Deal Amount (number)
    max( CASE WHEN key = 'recent_deal_close_date' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS recent_deal_close_date, -- Recent Deal Close Date (timestamp)
    max( CASE WHEN key = 'hs_sales_email_last_replied' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_sales_email_last_replied, -- Recent Sales Email Replied Date (timestamp)
    max( CASE WHEN key = 'state' THEN value END ) AS state, -- State/Region (string)
    max( CASE WHEN key = 'address' THEN value END ) AS address, -- Street Address (string)
    max( CASE WHEN key = 'address2' THEN value END ) AS address2, -- Street Address 2 (string)
    max( CASE WHEN key = 'hs_analytics_first_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_first_timestamp, -- Time First Seen (timestamp)
    max( CASE WHEN key = 'hs_analytics_last_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_last_timestamp, -- Time Last Seen (timestamp)
    max( CASE WHEN key = 'timezone' THEN value END ) AS timezone, -- Time Zone (string)
    max( CASE WHEN key = 'hs_analytics_first_visit_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_first_visit_timestamp, -- Time of First Session (timestamp)
    max( CASE WHEN key = 'hs_analytics_last_visit_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_last_visit_timestamp, -- Time of Last Session (timestamp)
    max( CASE WHEN key = 'total_money_raised' THEN value END ) AS total_money_raised, -- Total Money Raised (string)
    max( CASE WHEN key = 'total_revenue' THEN value END ) AS total_revenue, -- Total Revenue (number)
    max( CASE WHEN key = 'twitterhandle' THEN value END ) AS twitterhandle, -- Twitter Handle (string)
    max( CASE WHEN key = 'type' THEN value END ) AS type, -- Type (enumeration)
    max( CASE WHEN key = 'web_technologies' THEN value END ) AS web_technologies, -- Web Technologies (enumeration)
    max( CASE WHEN key = 'website' THEN value END ) AS website, -- Website URL (string)
    max( CASE WHEN key = 'founded_year' THEN value END ) AS founded_year -- Year Founded (string)
FROM hubspot_company_properties p
GROUP BY p.hubspot_company_id;
```

## Example: Company creation by month

```sql
SELECT
  date_trunc('month', createdate) create_month,
  count(distinct hubspot_companies_id) company_count
FROM hubspot_companies_report
GROUP BY create_month
ORDER BY create_month;
```
