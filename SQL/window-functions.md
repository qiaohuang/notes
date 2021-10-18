# Modern SQL Window Function

Link: <https://www.windowfunctions.com/>

## Over

Over allows us to break down our aggregate functions, it is often used for running totals.

### Partition by

Partition by allows us to further subdivide the preceding Over Command, it is often used for running totals of types or classes of iterms.

### Preceding and Following

Preceding and Following allow us to perform aggregate functions on the rows just before and after the current row.

```sql
select name, sum(weight) over (order by weight DESC ROWS between unbounded preceding and current row) as running_total_weight from cats order by running_total_weight
```

Use Unbounded Preceding to make sure you don't include extra rows if 2 rows evaluate to the same thing.

## Ranking

| name | time | row_number| rank | dense_rank |
| ---- | ---- | ----------| ---- | ---------- |
| andy | 101  | 1         | 1    | 1          |
| bob  | 103  | 2         | 2    | 2          |
| cedric | 104 | 3         | 3    | 3         |
| dave | 104  | 4         | 3    | 3          |
| eric | 108  | 5         | 5    | 4          |

| name | time | percent_rank | cume_dist |
| ---- | ---- | ------------ | --------- |
| andy | 101  | 0            | 0.2       |
| bob  | 103  | 0.25         | 0.4       |
| cedric | 104 | 0.5         | 0.8       |
| dave | 104  | 0.5          | 0.8       |
| eric | 108  | 1            | 1         |

```sql
select row_number() over (order by color,name) as unique_number, name, color from cats

select row_number() over (partition by color order by name) as unique_number, name, color from cats
```
