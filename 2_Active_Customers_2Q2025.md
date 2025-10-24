# 2025 2Q Active Customers Query
> These queries identifies distinct users who placed orders in the second quarter of 2025 within the thelook_ecommerce dataset. It joins the `users` and `orders` tables to link users with their purchase activity and then aggregates the results by year and quarter.  
## 1st task:
*   Calculate Total of distinct Registered Users in the eLook Database.
*   This will be done by using a distinct count on the table.
```sql
select
  count(distinct U.id) AS user_id,  
from
  bigquery-public-data.thelook_ecommerce.users U
```

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

|index|year\_quarter|orders|user\_id|average\_order\_by\_act\_cust|
|---|---|---|---|---|
|0|2025-2Q|12795|11606|1\.1|

## Insigths
  * Out of 100,000 registered customers, 11,355 made purchases in Q2 2025.
  * On average, active customers placed 1.09 orders during this period.
  * Only 104 active customers made 3 or 4 purchases in Q2 2025.
  * Efforts are needed to increase the purchase frequency among active customers. To this intent a report was made for the marketing teams to analise and study the case (see last query).

## Extra Queries for more Business Context
  * General overview of the Q22025 Operations.

```sql
select
  count(distinct(O.order_id)) AS count_distinct_order_id,
  count(distinct U.id) AS count_distinct_user_id,
  round(count(distinct(O.order_id)) / count(distinct U.id),0)
   AS avg_orders_user_id ,    
  sum(num_of_item) AS sum_number_of_items ,  
  min(num_of_item) AS min_number_of_items ,
  max(num_of_item) AS max_number_of_items,
  round(avg (num_of_item),1)
   AS avg_number_of_items,  
from
  bigquery-public-data.thelook_ecommerce.users U
join
  bigquery-public-data.thelook_ecommerce.orders O
on
  U.id = O.user_id
where
  extract(year from (O.created_at)) = 2025 and
  extract(quarter from (O.created_at)) = 2
```
|index|count\_distinct\_order\_id|count\_distinct\_user\_id|avg\_orders\_user\_id|sum\_number\_of\_items|min\_number\_of\_items|max\_number\_of\_items|avg\_number\_of\_items|
|---|---|---|---|---|---|---|---|
|0|12416|11355|1\.1|18057|1|4|1\.5|

* Breakdown by Quantity of orders made by active customers in Q22025.
```sql
select
  count_distinct_order_id,
  sum(count_distinct_user_id)
from
(
select
  user_id,
  count(distinct(O.order_id)) AS count_distinct_order_id,
  count(distinct U.id) AS count_distinct_user_id,
  count(distinct(O.order_id)) / count(distinct U.id) AS avg_orders_user_id ,    
  sum(num_of_item) AS sum_number_of_items ,  
  min(num_of_item) AS min_number_of_items ,
  max(num_of_item) AS max_number_of_items,
  avg (num_of_item)AS avg_number_of_items,  
from
  bigquery-public-data.thelook_ecommerce.users U
join
  bigquery-public-data.thelook_ecommerce.orders O
on
  U.id = O.user_id
where
  extract(year from (O.created_at)) = 2025 and
  extract(quarter from (O.created_at)) = 2
group by
  1
)
group by
1
```
|index|count\_distinct\_order\_id|f0\_|
|---|---|---|
|0|1|10403|
|1|2|848|
|2|3|99|
|3|4|5|

* Report to provide support to marketing team in order to rise the number of orders by active customers. 
```sql
%%bigquery --project {project_id}
select
  user_id,
  count(distinct(O.order_id)) AS count_distinct_order_id,
  count(distinct U.id) AS count_distinct_user_id,
  count(distinct(O.order_id)) / count(distinct U.id) AS avg_orders_user_id ,    
  sum(num_of_item) AS sum_number_of_items ,  
  min(num_of_item) AS min_number_of_items ,
  max(num_of_item) AS max_number_of_items,
  avg (num_of_item)AS avg_number_of_items,  
from
  bigquery-public-data.thelook_ecommerce.users U
join
  bigquery-public-data.thelook_ecommerce.orders O
on
  U.id = O.user_id
where
  extract(year from (O.created_at)) = 2025 and
  extract(quarter from (O.created_at)) = 2
group by
  1
```

|index|user\_id|count\_distinct\_order\_id|count\_distinct\_user\_id|avg\_orders\_user\_id|sum\_number\_of\_items|min\_number\_of\_items|max\_number\_of\_items|avg\_number\_of\_items|
|---|---|---|---|---|---|---|---|---|
|0|39030|1|1|1\.0|1|1|1|1\.0|
|1|77646|1|1|1\.0|1|1|1|1\.0|
|2|58267|1|1|1\.0|1|1|1|1\.0|
|3|60359|1|1|1\.0|1|1|1|1\.0|
|4|3277|1|1|1\.0|1|1|1|1\.0|
|...|...|...|...|...|...|...|...|...|
|...|...|...|...|...|...|...|...|...|
|11352|	75524|	1	|1	|1.0	|4|	4	|4|	4.0
|11353|	24052|	1	|1	|1.0|	4	|4|	4|	4.0
|11354|	12090	|1	|1	|1.0	|4	|4|	4	|4.0
