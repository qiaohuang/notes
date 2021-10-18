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

`rank()` lets us index the ordered items. Unlike `row_number()` it allows duplicate numbers. `dense_rank()` differs from `rank()` as it increases sequentially.

`percent_rank()` scores everything from 0-1 allowing us to generate distributions or percentiles. `cume_dist()` is similar to `percent_rank()`. The difference is the first entry in cume_dist is not 0.

## Grouping

```sql
select name, time,
lag(time, 1) over (order by time) as time_of_person_infront_of_me,
lead(time, 1) over (order by time) - time as how_much_i_was_infront_of_ther_person_behind_me
FROM runners order by time

select name, time,
nth_value(time, 3) over (order by time) - time as to_go_faster_to_make_podium,
nth_value(time, 2) over (order by time RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as time_of_second_runner,
ntile(2) over (order by time) as which_half
FROM runners order by time;

select name, weight, coalesce(weight - lag(weight, 1) over (order by weight), 0) as weight_to_lose FROM cats order by weight

select name, breed, weight, coalesce(weight - lag(weight, 1) over (partition by breed order by weight), 0) as weight_to_lose from cats order by weight, name

select name, weight, breed, coalesce(cast(lead(weight, 1) over (partition by breed order by weight) as varchar), 'fattest cat') as next_heaviest from cats order by weight
-- lead() can also work inside a cast

select name, weight, coalesce(nth_value(weight, 4) over (order by weight), 99.9) as imagined_weight from cats order by weight

select distinct(breed), nth_value(weight, 2) over ( partition by breed order by weight RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING ) as imagined_weight from cats order by breed;
```

`first_value()` (and `nth_value()`) allow us to select the first value of a subgroup.

## Others

### Filter

```sql
select breed, avg(weight) as average_weight, avg(weight) filter (where age > 1) average_old_weight from cats group by breed order by breed
```

### Array Agg

```sql
select color, array_agg(name) as names from cats group by color order by color desc
```

`array_agg()` allows us to group our data together.

### Window

```sql
select name, weight, ntile(2) over ntile_window as by_half, ntile(3) over ntile_window as thirds, ntile(4) over ntile_window as quart from cats window ntile_window AS ( ORDER BY weight) order by weight, name
```

Use of Window can simplify some of our complex queries.
