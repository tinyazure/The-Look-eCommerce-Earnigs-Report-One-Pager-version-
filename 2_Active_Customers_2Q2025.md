# 2025 2Q Active Customers Query
This query identifies distinct users who placed orders in the second quarter of 2025 within the thelook_ecommerce dataset. It joins the `users` and `orders` tables to link users with their purchase activity and then aggregates the results by year and quarter.  
## 1st task:
  * Calculate Total of distinct Registered Users
  * Will be used the distinct count on the table users
```sql
select
  count(distinct U.id) AS user_id,  
from
  bigquery-public-data.thelook_ecommerce.users U
```
## OutPut:
|index|distinct_count_user_id|
|---|---|
|0|100000|

## 2nd task:
  * The output of the last query only give us the total registered users.
  * Need to know the users who made purchases on the 2Q 2025 period.
  * Will be used Join Functions
```sql
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
## Output ##
|index|user\_id|orders|year\_quarter|
|---|---|---|---|
|0|31|1|2025-2Q|
|1|32|1|2025-2Q|
|2|40|1|2025-2Q|
|---|---|---|---|
|11537|99990|1|2025-2Q|
|11538|99993|1|2025-2Q|



##2nd task
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

## Output

|index|count\_user\_id|year\_quarter|
|---|---|---|
|0|11539|2025-2Q|

## Insigths
Cuantos total registered??
