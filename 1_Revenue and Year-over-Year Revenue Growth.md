# 2Q 2025 Revenue and Year-over-Year Revenue Grow <br>

This queries calculates key metrics including net revenue, order count, and average order value for Q2 of 2024 and 2025 from the order_items table. It filters for orders with 'Complete', 'Shipped', or 'Processing' statuses within the specified quarters and years. The results are presented by year and quarter, providing a comparative view of performance between the two periods.<br>

## 1st Task:
  * Calculate Q2 2025 Net Revenue Total Net Revenue
  * Also we will add below the and it´s breakdown using GROUP BY, WHERE, clause and Time format Functions Using UNION ALL
    
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



```
