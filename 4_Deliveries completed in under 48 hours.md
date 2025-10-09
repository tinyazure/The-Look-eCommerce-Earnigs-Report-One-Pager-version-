# 2025Q2 Deliveries in within 48 hours Query
This query counts the number of orders completed or returned in the second quarter of 2025 that were delivered within 48 hours. It groups the results by year and quarter, using the orders table from the look_ecommerce dataset.

## 1st Task:
  * Calculate the list of all orders during the Q2 2025 period.
```sql
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

## OutPut:
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
```sql
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
## OutPut:
| year_quarter | Count_order_id | 
|--------------|----------|
| 2025-2Q      | 610   |
## Insights
  * 13% <=48 de total ordenes despachadas
