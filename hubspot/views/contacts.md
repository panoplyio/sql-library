# Hubspot Contacts Report

Instructions | Details
---|---
Description| Denormalized view of Hubspot's default values in `hubspot_contacts_properties` key-value pair table
Usage| This is the "ready for analysis" view of Hubspot contacts
Modifications| Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name. Dates are formatted as Unix timestamps so they must be transformed and cast into dates.

```sql
-- CREATE VIEW hubspot_contacts_report AS
SELECT 	
  -- CONTACT DETAILS --
	p.hubspot_contacts_id,
	max( CASE WHEN key = 'hs_object_id' THEN value END ) AS hs_object_id, -- Contact ID (number)
	max( CASE WHEN key = 'email' THEN value END ) AS email, -- Email (string)
	max( CASE WHEN key = 'hs_email_domain' THEN value END ) AS hs_email_domain, -- Email Domain (string)
	max( CASE WHEN key = 'firstname' THEN value END ) AS firstname, -- First Name (string)
	max( CASE WHEN key = 'lastname' THEN value END ) AS lastname, -- Last Name (string)
	max( CASE WHEN key = 'twitterhandle' THEN value END ) AS twitterhandle, -- Twitter Username (string)
	max( CASE WHEN key = 'createdate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS createdate, -- Create Date (timestamp)
	max( CASE WHEN key = 'lastmodifieddate' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS lastmodifieddate, -- Last Modified Date (timestamp)
	max( CASE WHEN key = 'hs_lifecyclestage_lead_date' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_lifecyclestage_lead_date, -- Became a Lead Date (timestamp)
	max( CASE WHEN key = 'city' THEN value END ) AS city, -- City (string)
	max( CASE WHEN key = 'state' THEN value END ) AS state, -- State/Region (string)
	max( CASE WHEN key = 'jobtitle' THEN value END ) AS jobtitle, -- Job Title (string)
	max( CASE WHEN key = 'lifecyclestage' THEN value END ) AS lifecyclestage, -- Lifecycle Stage (enumeration)
	max( CASE WHEN key = 'company' THEN value END ) AS company, -- Company Name (string)
	max( CASE WHEN key = 'website' THEN value END ) AS website, -- Website URL (string)
	max( CASE WHEN key = 'hs_is_contact' THEN value END ) AS hs_is_contact, -- Is a contact (bool)
	max( CASE WHEN key = 'hs_all_contact_vids' THEN value END ) AS hs_all_contact_vids, -- All vids for a contact (enumeration)
	max( CASE WHEN key = 'hs_predictivecontactscore_v2' THEN value END ) AS hs_predictivecontactscore_v2, -- Likelihood to close (number)
  -- EMAIL DETAILS --
	max( CASE WHEN key = 'hs_email_quarantined' THEN value END ) AS hs_email_quarantined, -- Email Address Quarantined (bool)
	max( CASE WHEN key = 'hs_email_recipient_fatigue_recovery_time' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_email_recipient_fatigue_recovery_time, -- Email Address Recipient Fatigue Next Available Sending Time (timestamp)
	max( CASE WHEN key = 'hs_email_optout_7352524' THEN value END ) AS hs_email_optout_7352524, -- Opted out of email: Customer Service Communication (enumeration)
	max( CASE WHEN key = 'hs_email_optout' THEN value END ) AS hs_email_optout, -- Unsubscribed from all email (bool)
  -- CONVERSION DETAILS --
	max( CASE WHEN key = 'num_conversion_events' THEN value END ) AS num_conversion_events, -- Number of Form Submissions (number)
	max( CASE WHEN key = 'num_unique_conversion_events' THEN value END ) AS num_unique_conversion_events, -- Number of Unique Forms Submitted (number)
  -- ANALYTICS DETAILS --
	max( CASE WHEN key = 'hs_analytics_first_touch_converting_campaign' THEN value END ) AS hs_analytics_first_touch_converting_campaign, -- First Touch Converting Campaign (string)
	max( CASE WHEN key = 'hs_analytics_last_touch_converting_campaign' THEN value END ) AS hs_analytics_last_touch_converting_campaign, -- Last Touch Converting Campaign (string)
	max( CASE WHEN key = 'hs_analytics_num_page_views' THEN value END ) AS hs_analytics_num_page_views, -- Number of Pageviews (number)
	max( CASE WHEN key = 'hs_analytics_num_visits' THEN value END ) AS hs_analytics_num_visits, -- Number of Sessions (number)
	max( CASE WHEN key = 'hs_analytics_num_event_completions' THEN value END ) AS hs_analytics_num_event_completions, -- Number of event completions (number)
	max( CASE WHEN key = 'hs_analytics_first_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_first_timestamp, -- Time First Seen (timestamp)
	max( CASE WHEN key = 'hs_analytics_first_visit_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_first_visit_timestamp, -- Time of First Session (timestamp)
	max( CASE WHEN key = 'hs_analytics_last_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_last_timestamp, -- Time Last Seen (timestamp)
	max( CASE WHEN key = 'hs_analytics_last_visit_timestamp' THEN timestamp 'epoch' + value / 1000 * interval '1 second' END ) AS hs_analytics_last_visit_timestamp, -- Time of Last Session (timestamp)
	max( CASE WHEN key = 'hs_analytics_source' THEN value END ) AS hs_analytics_source, -- Original Source (enumeration)
	max( CASE WHEN key = 'hs_analytics_source_data_1' THEN value END ) AS hs_analytics_source_data_1, -- Original Source Drill-Down 1 (string)
	max( CASE WHEN key = 'hs_analytics_source_data_2' THEN value END ) AS hs_analytics_source_data_2, -- Original Source Drill-Down 2 (string)
	max( CASE WHEN key = 'hs_analytics_average_page_views' THEN value END ) AS hs_analytics_average_page_views, -- Average Pageviews (number)
	max( CASE WHEN key = 'hs_analytics_revenue' THEN value END ) AS hs_analytics_revenue -- Event Revenue (number)
FROM hubspot_contacts_properties p
GROUP BY p.hubspot_contacts_id
ORDER BY createdate DESC
```

## Example: Contact creation by month

```sql
SELECT
  date_trunc('month', createdate) create_month,
  count(distinct hubspot_contacts_id) contact_count
FROM hubspot_contacts_report
GROUP BY create_month
ORDER BY create_month;
```
