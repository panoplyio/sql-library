---
title: Deals consolidated
description: Denormalized view of Hubspot's deals data. This view can be used together with other Hubspot views
usage: This is the "ready for analysis" view of Hubspot deals
modifications: Properties can be removed or added. To add custom properties, refer to your Hubspot properties setting for their name. Dates are formatted as Unix timestamps so they must be transformed and cast into dates.
---

```sql
-- CREATE VIEW hubspot_deals_consolidated AS
SELECT hdp.*
FROM (
	SELECT id AS deal_id
	FROM hubspot_deals
	WHERE isdeleted IS FALSE
	) hd
INNER JOIN (
	SELECT hubspot_deals_id as deal_id
		-- Deal Details
		, MAX(CASE WHEN KEY = 'dealname' THEN value END) AS deal_name
		, MAX(CASE WHEN KEY = 'dealtype' THEN value END) AS deal_type
		, MAX(CASE WHEN KEY = 'dealstage' THEN value END) AS deal_stage
		, MAX(CASE WHEN KEY = 'description' THEN value END) AS description
		-- Dates and Durations
		, MAX(CASE WHEN KEY = 'createdate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS created_date
		, MAX(CASE WHEN KEY = 'hs_date_entered_appointmentscheduled' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_entered_appointmentscheduled
		, MAX(CASE WHEN KEY = 'hs_date_exited_appointmentscheduled' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_exited_appointmentscheduled
		, MAX(CASE WHEN KEY = 'hs_time_in_appointmentscheduled' THEN EXTRACT(epoch from value / 1000 * interval '1 second') END) AS time_in_appointmentscheduled --type interval
		, MAX(CASE WHEN KEY = 'hs_date_entered_qualifiedtobuy' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_entered_qualifiedtobuy
		, MAX(CASE WHEN KEY = 'hs_date_exited_qualifiedtobuy' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_exited_qualifiedtobuy
		, MAX(CASE WHEN KEY = 'hs_time_in_qualifiedtobuy' THEN EXTRACT(epoch from value / 1000 * interval '1 second') END) AS time_in_qualifiedtobuy --type interval
		, MAX(CASE WHEN KEY = 'hs_date_entered_presentationscheduled' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_entered_presentationscheduled
		, MAX(CASE WHEN KEY = 'hs_date_exited_presentationscheduled' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_exited_presentationscheduled
		, MAX(CASE WHEN KEY = 'hs_time_in_presentationscheduled' THEN EXTRACT(epoch from value / 1000 * interval '1 second') END) AS time_in_presentationscheduled --type interval
		, MAX(CASE WHEN KEY = 'hs_date_entered_closedlost' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_entered_closedlost
		, MAX(CASE WHEN KEY = 'hs_time_in_closedlost' THEN EXTRACT(epoch from value / 1000 * interval '1 second') END) AS time_in_closedlost --type interval
		, MAX(CASE WHEN KEY = 'hs_date_entered_closedwon' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS date_entered_closedwon
		, MAX(CASE WHEN KEY = 'hs_time_in_closedwon' THEN EXTRACT(epoch from value / 1000 * interval '1 second') END) AS time_in_closedwon --type interval
		, MAX(CASE WHEN KEY = 'closedate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS close_date
		, MAX(CASE WHEN KEY = 'days_to_close' THEN value END) AS days_to_close
		, MAX(CASE WHEN KEY = 'engagements_last_meeting_booked' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_meeting_booked
		, MAX(CASE WHEN KEY = 'hs_latest_meeting_activity' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS latest_meeting_activity
		, MAX(CASE WHEN KEY = 'hs_lastmodifieddate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS last_modified_date
		, MAX(CASE WHEN KEY = 'hubspot_owner_assigneddate' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS owner_assigned_date
		, MAX(CASE WHEN KEY = 'hs_sales_email_last_replied' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS sales_email_last_replied
		, MAX(CASE WHEN KEY = 'notes_last_contacted' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS notes_last_contacted
		, MAX(CASE WHEN KEY = 'notes_last_updated' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS notes_last_updated
		, MAX(CASE WHEN KEY = 'notes_next_activity_date' THEN TIMESTAMP 'epoch' + value / 1000 * interval '1 second' END) AS notes_next_activity_date
		-- Sales Pipeline Info
		, MAX(CASE WHEN KEY = 'pipeline' THEN value END) AS pipeline
		, MAX(CASE WHEN KEY = 'hubspot_owner_id' THEN CAST(value as INT) END) AS owner_id
        , MAX(CASE WHEN KEY = 'hs_all_owner_ids' THEN value END) AS all_owner_ids
		, MAX(CASE WHEN KEY = 'closed_lost_reason' THEN value END) AS closedlost_reason
		, MAX(CASE WHEN KEY = 'closed_won_reason' THEN value END) AS closedwon_reason
		, MAX(CASE WHEN KEY = 'hs_is_closed' THEN value END) AS is_closed
		-- Money
		, MAX(CASE WHEN KEY = 'amount_in_home_currency' THEN value END) AS amount_in_home_currency
		, MAX(CASE WHEN KEY = 'amount' THEN value END) AS amount
		, MAX(CASE WHEN KEY = 'hs_closed_amount' THEN value END) AS closed_amount
        , MAX(CASE WHEN KEY = 'hs_closed_amount_in_home_currency' THEN value END) AS closed_amount_in_home_currency
		, MAX(CASE WHEN KEY = 'hs_projected_amount' THEN value END) AS projected_amount
        , MAX(CASE WHEN KEY = 'hs_projected_amount_in_home_currency' THEN value END) AS projected_amount_in_home_currency
		, MAX(CASE WHEN KEY = 'hs_forecast_amount' THEN value END) AS forecast_amount
		-- Probabilities
		, MAX(CASE WHEN KEY = 'hs_deal_stage_probability' THEN value END) AS deal_stage_probability
		, MAX(CASE WHEN KEY = 'hs_forecast_probability' THEN value END) AS forecast_probability
		, MAX(CASE WHEN KEY = 'hs_manual_forecast_category' THEN value END) AS manual_forecast_category
		-- Data Fields
		, MAX(CASE WHEN KEY = 'hs_analytics_source' THEN value END) AS source
		, MAX(CASE WHEN KEY = 'hs_analytics_source_data_1' THEN value END) AS source_data_1
		, MAX(CASE WHEN KEY = 'hs_analytics_source_data_2' THEN value END) AS source_data_2
		, MAX(CASE WHEN KEY = 'lead_source' THEN value END) AS lead_source
		, MAX(CASE WHEN KEY = 'lod' THEN value END) AS lod -- unsure what this is
		-- Numbers
		, MAX(CASE WHEN KEY = 'num_associated_contacts' THEN value END) AS num_associated_contacts
		, MAX(CASE WHEN KEY = 'num_contacted_notes' THEN value END) AS num_contacted_notes
		, MAX(CASE WHEN KEY = 'num_notes' THEN value END) AS num_notes
	FROM hubspot_deals_properties
	GROUP BY hubspot_deals_id
	) hdp ON hdp.deal_id = hd.deal_id;
```
