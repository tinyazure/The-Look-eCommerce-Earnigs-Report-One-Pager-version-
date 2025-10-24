# 2025Q2 Deliveries in within 48 hours Query
These queries counts the number of orders completed or returned in the second quarter of 2025 that were delivered within 48 hours. Also groups the results by year and quarter, using the orders table from the look_ecommerce dataset in order to get aggregations.

## 1st Task:
 * Retrieve unique orders with 'Complete' or 'Returned' status.
 * Calculate the delivery time in hours for each order using the difference between delivered and created dates.
 * Filter for orders with a delivery time of 48 hours or less.
```sql
#Delivery time in hours breakdown by orders_id for the Q22025 period.
select
  format_datetime("%Y-%QQ",created_at)as year_quarter,
  order_id as order_id,
  created_at as created_at,
  delivered_at as delivered_at,
  TIMESTAMP_DIFF(delivered_at,created_at, HOUR) AS delivery_time
from
`bigquery-public-data.thelook_ecommerce.orders`
where
  status in ('Complete','Returned') and
  EXTRACT(QUARTER FROM created_at) = 2 and
  extract(YEAR FROM created_at) = 2025 
order by
  created_at asc
```
| year_quarter | order_id | created_at                | delivered_at              | delivery_time |
|--------------|----------|--------------------------|---------------------------|---------------|
| 2025-2Q      | 109364   | 2025-04-01 00:31:00+00:00| 2025-04-03 00:05:00+00:00 | 47            |
| 2025-2Q      | 98363    | 2025-04-01 00:50:00+00:00| 2025-04-03 15:40:00+00:00 | 62            |
| 2025-2Q      | 22021    | 2025-04-01 01:01:00+00:00| 2025-04-06 13:35:00+00:00 | 132           |
| 2025-2Q      | 21899    | 2025-04-01 01:15:00+00:00| 2025-04-03 07:45:00+00:00 | 54            |
| 2025-2Q      | 50629    | 2025-04-01 01:23:00+00:00| 2025-04-03 11:48:00+00:00 | 58            |
| ...          | ...      | ...                      | ...                       | ...           |
| 2025-2Q      | 74729    | 2025-06-30 17:37:00+00:00| 2025-07-02 21:08:00+00:00 | 51            |
| 2025-2Q      | 120255   | 2025-06-30 17:48:00+00:00| 2025-07-04 19:53:00+00:00 | 98            |
| 2025-2Q      | 101014   | 2025-06-30 17:59:00+00:00| 2025-07-04 05:08:00+00:00 | 83            |
| 2025-2Q      | 123833   | 2025-06-30 18:13:00+00:00| 2025-07-04 22:23:00+00:00 | 100           |
| 2025-2Q      | 33526    | 2025-06-30 18:56:00+00:00| 2025-07-07 01:13:00+00:00 | 150           |
4635 rows × 5 columns

## 2nd Task:
 * Create a subquery to get agregations.
 * Count the number of orders that meet the criteria from the first task.
 * Group the counts by year and quarter.
```sql
#Delivery time in hours aggregated level for Q22025 period.
select
year_quarter as year_quarter,
count(order_id) as count_order_id,
from
(
select
  format_datetime("%Y-%QQ",created_at)as year_quarter,
  order_id as order_id,
  created_at as created_at,
  delivered_at as delivered_at,
  TIMESTAMP_DIFF(delivered_at,created_at, HOUR) AS delivery_time
from
`bigquery-public-data.thelook_ecommerce.orders`
where
  status in ('Complete','Returned') and
  EXTRACT(QUARTER FROM created_at) = 2 and
  extract(YEAR FROM created_at) = 2025 and
  TIMESTAMP_DIFF(delivered_at,created_at, HOUR) <= 48
order by
  created_at asc
)
group by year_quarter
order by year_quarter
```
| year_quarter | Count_order_id | 
|--------------|----------|
| 2025-2Q      | 610   |

## Insights
Based on the analysis of 2025 Q2 orders with 'Complete' or 'Returned' status:

*   Out of 4266 total orders, 631 orders were delivered within 48 hours, representing approximately 14.8% of the total.
*   The average delivery time for all 4266 orders in 2025 Q2 was 96 hours (or 3.5  days).
*   2166 orders were delivered above the average delivery time of 96 hours, with an average delivery time of 129 hours (or 5.4 days).
*   It is recommended to investigate the operational processes for the 2166 orders with delivery times exceeding the average to identify areas for improvement and reduce delivery times.

## Extra Queries for more Insghts and Business Context
  * Calculate the delivery Average time for the period.
  * Calculate the count of delivered orderes above the average.
```sql
select 
  count(distinct order_id) as total_orders,
  round(AVG(delivery_time_hours),0) as avg_delivery_hours,
  round(AVG(delivery_time_days),1) as avg_delivery_days
from
  (
  select
    format_datetime("%Y-%QQ",created_at)as year_quarter,
    order_id as order_id,
    created_at as created_at,
    delivered_at as delivered_at,
    TIMESTAMP_DIFF(delivered_at,created_at, HOUR) AS delivery_time_hours,
    TIMESTAMP_DIFF(delivered_at,created_at, DAY) AS delivery_time_days
  from
  `bigquery-public-data.thelook_ecommerce.orders`
  where
    status in ('Complete','Returned') and
    EXTRACT(QUARTER FROM created_at) = 2 and
    extract(YEAR FROM created_at) = 2025 #and#
    #TIMESTAMP_DIFF(delivered_at,created_at, HOUR) <= 48#
  order by
    created_at asc
  )
```
|index|total\_orders|avg\_delivery\_hours|avg\_delivery\_days|
|---|---|---|---|
|0|4266|96\.0|3\.5|

```sql
%%bigquery --project {project_id}
select 
  count(distinct order_id) as total_orders,
  round(AVG(delivery_time_hours),0) as avg_delivery_hours,
  round(AVG(delivery_time_days),1) as avg_delivery_days
from
  (
  select
    format_datetime("%Y-%QQ",created_at)as year_quarter,
    order_id as order_id,
    created_at as created_at,
    delivered_at as delivered_at,
    TIMESTAMP_DIFF(delivered_at,created_at, HOUR) AS delivery_time_hours,
    TIMESTAMP_DIFF(delivered_at,created_at, DAY) AS delivery_time_days
  from
  `bigquery-public-data.thelook_ecommerce.orders`
  where
    status in ('Complete','Returned') and
    EXTRACT(QUARTER FROM created_at) = 2 and
    extract(YEAR FROM created_at) = 2025 
  order by
    created_at asc
  )
  where 
  delivery_time_hours >= 96
```
|index|total\_orders|avg\_delivery\_hours|avg\_delivery\_days|
|---|---|---|---|
|0|2166|129\.0|4\.9|

