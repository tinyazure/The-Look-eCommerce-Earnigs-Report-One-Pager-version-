# 2025 2Q Active Customers Query
This query identifies distinct users who placed orders in the second quarter of 2025 within the thelook_ecommerce dataset. It joins the `users` and `orders` tables to link users with their purchase activity and then aggregates the results by year and quarter.  
## 1st task:
*   Calculate Total of distinct Registered Users in the eLook Database.
*   This will be done by using a distinct count on the table.
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
  * The output of the previous query only pulls the total registered users (it´s not equals to active).
  * Need to know the registered users who made purchases in the 2Q 2025 period(active customers). This will be done by using a left join with the orders table to count the orders made by each user in Q2 2025.
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



## 3rd Task:
  * Aggregate the results from the previous query to get the total number of active customers and orders in 2Q 2025 using subquery.
  * Average number of orders made by each active customer in that period will be added for more insights and context.
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

|index|year\_quarter|orders|user\_id|average\_order\_by\_act\_cust|
|---|---|---|---|---|
|0|2025-2Q|12795|11606|1\.1|

## Insigths
 * From 100K registered customers, 11.5k made purchases on Q2 2025.
 * In average Active customers made a 1.10 purchase in the period. 
