# 2Q 2025 Revenue and Year-over-Year Revenue Grow <br>

This queries calculates key metrics including net revenue, order count, and average order value for Q2 of 2024 and 2025 from the order_items table. It filters for orders with 'Complete', 'Shipped', or 'Processing' statuses within the specified quarters and years. The results are presented by year and quarter, providing a comparative view of performance between the two periods.<br>

## 1st Task:
  * Calculate Q2 2025 Net Revenue Total Net Revenue
  * Also we will add below the and it´s breakdown using GROUP BY, WHERE, clause and Date/Time format Functions format Functions Using UNION ALL
    
```sql
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
## OutPut:
| index | year_quarter      | order_status | net_revenue |
|-------|------------------|--------------|-------------|
| 1     | 2025-2Q          | Processing   | 227759.43   |
| 3     | 2025-2Q          | Complete     | 287994.35   |
| 2     | 2025-2Q          | Shipped      | 334935.89   |
| 0     | Total Net Revenue|              | 850689.67   |

## 2nd Task:
 * Calculate 2Q 2025 and 2Q 2024 Total Net Revenue
 * Calculate for each period, count orders and average order value sing GROUP BY, WHERE, clauses, count command and Date/Time format Functions, and UNION ALL

```sql
select
  FORMAT_DATETIME('%Y-%QQ', created_at) as year_quarter,
  round(sum(sale_price),2)  as net_revenue,
  count(distinct(order_id)) as count_order_id,
  round(round(sum(sale_price),2) / count(distinct(order_id)),2) as average_order_value
  from
  bigquery-public-data.thelook_ecommerce.order_items
where
  extract(QUARTER FROM created_at) = 2 and
  extract(YEAR FROM created_at) = 2025 and
  status IN ('Complete','Shipped', 'Processing')
group by
  year_quarter

UNION ALL
select
  FORMAT_DATETIME('%Y-%QQ', created_at) as year_quarter,
  round(sum(sale_price),2)  as net_revenue,
  count(distinct(order_id)) as count_order_id,
  round(round(sum(sale_price),2) / count(distinct(order_id)),2) as average_order_value
from
  bigquery-public-data.thelook_ecommerce.order_items
where
  extract(QUARTER FROM created_at) = 2 and
  extract(YEAR FROM created_at) = 2024 and
  status IN ('Complete','Shipped', 'Processing')
group by
  year_quarter
```

## OutPut:
| index | year_quarter | net_revenue | count_order_id | average_order_value |
|-------|--------------|-------------|---------------|--------------------|
| 0     | 2024-2Q      | 465285.61   | 5440          | 85.53              |
| 1     | 2025-2Q      | 850689.67   | 10100         | 84.23              |



## Insights:
 * The net Revenue Growth is 82% (+ u$ 0.385 m). This is explained on more Quantities of orders and purchases received. Prices are relative flat YoY. Recommended to the UX team streamline the web interaction looking to increase the ticket with sales up. 
 
 * Shipped and Procceded makes the 66% of the net revenue. This represents significant risks on futher returned and cancelled orders. It´s recommended for the oppertations teams to close as fast the received orders for example avoiding stocks out in theis wharehouses and negotiate with suppliers for better lead times.







