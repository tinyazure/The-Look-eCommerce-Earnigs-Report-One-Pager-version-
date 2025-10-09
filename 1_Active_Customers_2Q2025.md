# 2025 2Q Active Customers Query
This query identifies distinct users who placed orders in the second quarter of 2025 within the thelook_ecommerce dataset. It joins the `users` and `orders` tables to link users with their purchase activity and then aggregates the results by year and quarter.
```sql
select
  count(user_id) as count_user_id,
  year_quarter as year_quarter
from
(
select
  distinct U.id AS user_id,
  count(distinct(O.order_id)) as orders,
  format_date("%Y-%QQ", O.created_at) as year_quarter
from
  bigquery-public-data.thelook_ecommerce.users U
left join
  bigquery-public-data.thelook_ecommerce.orders O
on
  U.id = O.user_id
where
  extract(year from (O.created_at)) = 2025 and
  extract(quarter from (O.created_at)) = 2
group by
1,3
ORDER BY
  user_id asc
)
group by
  year_quarter
```
|index|count\_user\_id|year\_quarter|
|---|---|---|
|0|11516|2025-2Q|
