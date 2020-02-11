# ﻿Monthly Cost and Conversions

Instructions | Details
---|---
Description | This query sums the total cost and conversions by month and year from the `adgroup_performance_report`.
Usage | This query can be used to create a line or bar chart of cost and conversions over time.
Modifications | The table in the `FROM` might need to be changed based on Schema and Destination settings in the data source.

```sql
SELECT EXTRACT(YEAR FROM "day") "year"
, EXTRACT(MONTH FROM "day") "month"
, ROUND(SUM(cost/1000000.0),2) cost -- devide cost by 1000000 to get Dollar since Google Provide Micro Dollar units
, SUM(conversions) conversions
FROM adwords_adgroup_performance_report -- Table name might be different based on Schema and Destination settings in the data source
GROUP BY 1,2
ORDER BY 1 ASC, 2 ASC
```

## Query Results Dictionary
Column | Description
---|---
`year`| Year extracted from the day column
`month`| Month extracted from the day column
`cost`| Total monthly cost in Dollars
`conversions`| Total monthly conversions
