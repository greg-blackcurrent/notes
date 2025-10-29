

```postgresql
select get_bucketed_energy(
       interval '1 month',
       'UTC',
       tstzrange('1/1/2025', now()),
       '{736}'
       );
```

```postgresql
with by_device_cte as (
    select
        time_bucket(interval '1 day', time, 'UTC') as time,
        last(generation, time) as generation,
        last("load", time) as "load",
        last(import, time) as import,
        last(export, time) as export,
        last(charging, time) as charging,
        last(discharging, time) as discharging,
        first(generation, time) as first_generation,
        first("load", time) as first_load,
        first(import, time) as first_import,
        first(export, time) as first_export,
        first(charging, time) as first_charging,
        first(discharging, time) as first_discharging
    from energyacc where cardinality(ARRAY[736])=0 OR location=ANY(ARRAY[736])
        and tstzrange('1/1/2025', now()) @> time
    group by 1, device
), cte as (
    select
        time,
        sum(generation) as generation,
        sum("load") as "load",
        sum(import) as import,
        sum(export) as export,
        sum(charging) as charging,
        sum(discharging) as discharging,
        sum(first_generation) as first_generation,
        sum(first_load) as first_load,
        sum(first_import) as first_import,
        sum(first_export) as first_export,
        sum(first_charging) as first_charging,
        sum(first_discharging) as first_discharging
    from by_device_cte
    group by time
), acc_cte as (
    select
        time,
        generation-LAG(generation, 1, first_generation) OVER (ORDER by time) as generation,
        load-LAG(load, 1, first_load) OVER (ORDER by time) as load,
        import-LAG(import, 1, first_import) OVER (ORDER by time) as import,
        export-LAG(export, 1, first_export) OVER (ORDER by time) as export,
        charging-LAG(charging, 1, first_charging) OVER (ORDER by time) as charging,
        discharging-LAG(discharging, 1, first_discharging) OVER (ORDER by time) as discharging
    from cte
    order by time asc
), nonacc_cte as (
    select
        time_bucket(interval '1 day', time, 'UTC') as time,
        sum(generation) as generation,
        sum("load") as "load",
        sum(import) as import,
        sum(export) as export,
        sum(charging) as charging,
        sum(discharging) as discharging
    FROM energy
    WHERE cardinality(ARRAY[736])=0 OR location=ANY(ARRAY[736])
        and tstzrange('1/1/2025', now()) @> time
    GROUP BY 1
)
select
    coalesce(AC.time, NA.time) as time,
    coalesce(AC.generation, 0) + coalesce(NA.generation, 0) as generation,
    coalesce(AC.load, 0) + coalesce(NA.load, 0) as "load",
    coalesce(AC.import, 0) + coalesce(NA.import, 0) as import,
    coalesce(AC.export, 0) + coalesce(NA.export, 0) as export,
    coalesce(AC.charging, 0) + coalesce(NA.charging, 0) as charging,
    coalesce(AC.discharging, 0) + coalesce(NA.discharging, 0) as discharging
from acc_cte AC
         full outer join nonacc_cte NA
                         on AC.time=NA.time
order by time desc;
```
