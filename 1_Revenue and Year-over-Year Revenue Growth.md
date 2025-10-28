# 2Q 2025 Revenue and Year-over-Year Revenue Grow <br>

> This query calculates net revenue, order count, and average order value for Q2 of 2024 and 2025 from the `order_items` table. It filters  orders with 'Complete', 'Shipped', or 'Processing' statuses within the specified quarters and years. The results are presented by year and quarter, providing a comparative view of performance between the two periods.
<br>

## 1st Task:
 * Calculate Q2 2025 Net Revenue by Order Status and the 2025 aggregated net revenue.
 * Using GROUP BY, WHERE clause and Date/Time format functions and UNION ALL, to get the aggregated overview in only one table.
    
```sql
select
  FORMAT_DATETIME('%Y-%QQ', created_at) as year_quarter,
  status as order_status,
  round(sum(sale_price),2)  as net_revenue,
from
  bigquery-public-data.thelook_ecommerce.order_items
where
  extract(QUARTER FROM created_at) = 2 and
  extract(YEAR FROM created_at) = 2025 and
  status IN ('Complete','Shipped', 'Processing')
group by
  year_quarter,
  status

UNION ALL

select
  "Total Net Revenue" as year_quarter,
  "" as order_status,
  round(sum(sale_price),2)  as net_revenue,
from
  bigquery-public-data.thelook_ecommerce.order_items
where
  extract(QUARTER FROM created_at) = 2 and
  extract(YEAR FROM created_at) = 2025 and
  status IN ('Complete','Shipped', 'Processing')

```  

|index|year\_quarter|order\_status|net\_revenue|
|---|---|---|---|
|0|Total Net Revenue||811853\.77|
|1|2025-2Q|Processing|206568\.27|
|2|2025-2Q|Complete|281970\.2|
|3|2025-2Q|Shipped|323315\.3|

## 2nd Task:
 * Calculate 2Q 2025 and 2Q 2024 at a agregated Total Net Revenue level.
 * Calculate for each period, count orders and average order value sing GROUP BY, WHERE, clauses, count command and Date/Time format Functions.

```sql
select
  FORMAT_DATETIME('%Y-%QQ', created_at) as year_quarter,
  round(sum(sale_price),2)  as net_revenue,
  count(distinct(order_id)) as count_order_id,
  round(round(sum(sale_price),2) / count(distinct(order_id)),2) as average_order_value
from
  bigquery-public-data.thelook_ecommerce.order_items
where
  status IN ('Complete','Shipped', 'Processing') and
  extract(QUARTER FROM created_at) = 2 and
  (extract(YEAR FROM created_at) = 2024 or extract(YEAR FROM created_at) = 2025) 
group by
  (year_quarter)
```

|index|year\_quarter|net\_revenue|count\_order\_id|average\_order\_value|
|---|---|---|---|---|
|0|2024-2Q|459723\.81|5379|85\.47|
|1|2025-2Q|811853\.77|9444|85\.97|


## Insights:
* Net revenue growth vs. Q22024 reached +82% (+$0.385 million), primarily driven by an increase in the number of orders and purchases. 
* Shipped and processed orders account for 66% of net revenue, which poses considerable risk for potential returns and cancellations. 
* Year-over-year prices remained relatively stable. It is recommended that the UX team optimize website interactions to encourage higher average order values and boost sales.
* The operations team is advised to expedite order fulfillment to minimize Shipped and processed times, minimize stockouts in warehouses, and negotiate improved lead times with suppliers.

## Expert Tip
> Instead of running aditional query using UNION all to append the Totals it´s good to use the rollup function
This would produce a sum of Sales Amount per each year quarter status combination as well, but also a row for year quarter sub-total and a grant total row for all regions.
```sql
select  
  FORMAT_DATETIME('%Y-%QQ', created_at) as year_quarter,
  status as order_status,
  round(sum(sale_price),2)  as net_revenue,
from
  bigquery-public-data.thelook_ecommerce.order_items
where
  status IN ('Complete','Shipped', 'Processing') and
  extract(QUARTER FROM created_at) = 2 and
  (extract(YEAR FROM created_at) = 2024 or extract(YEAR FROM created_at) = 2025) 
group by
  rollup(year_quarter, order_status)
```

|index|year\_quarter|order\_status|net\_revenue|
|---|---|---|---|
|0|||1219582\.77|
|1|2025-2Q||791298\.2|
|2|2024-2Q||428284\.57|
|3|2025-2Q|Processing|208176\.83|
|4|2025-2Q|Complete|263862\.53|
|5|2025-2Q|Shipped|319258\.84|
|6|2024-2Q|Processing|109275\.49|
|7|2024-2Q|Complete|140570\.86|
|8|2024-2Q|Shipped|178438\.22|


