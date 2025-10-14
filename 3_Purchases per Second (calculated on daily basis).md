# 2025Q2 Purchases per Day
This SQL query calculate the total purchases per day and purchases per hour by agregatting the total of different orders dividing by total of days in the period.

## 1st Task:
  * For the query will be used count agregations, distinct count and grouping by formated date.
```sql
select
  format_date("%Y-%QQ", created_at) as year_quarter,
  count(distinct(order_id)) as count_orders,
  count(distinct(date(created_at))) as count_days,
  round(count(distinct(order_id)) / count(distinct(date(created_at)))) as purchases_per_day,
  round((round(count(distinct(order_id)) / count(distinct(date(created_at))))/24)) as purchases_per_hour
from
  `bigquery-public-data.thelook_ecommerce.order_items`
where
  extract(QUARTER FROM created_at) = 2 AND
  extract(YEAR FROM created_at) = 2025
group by
  year_quarter
```

## OutPut:
|index|year\_quarter|count\_orders|count\_days|purchases\_per\_day|purchases\_per\_hour|
|---|---|---|---|---|---|
|0|2025-2Q|13649|91|150\.0|6\.0|

## Insight:
 * There are 6 purchases per Hour and 150 purchases a Day.
 * Knowing that the Average Ticket is about $85.97 (from the revenue query) we can deduce that the eLook in each hour generates $515 in Revenue.
