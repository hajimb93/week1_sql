docker -- help

------------------------------------------------------------
docker build --help 

Usage:  docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
      --add-host list           Add a custom host-to-IP mapping (host:ip)
      --build-arg list          Set build-time variables
      --cache-from strings      Images to consider as cache sources
      --cgroup-parent string    Optional parent cgroup for the container
      --compress                Compress the build context using gzip
      --cpu-period int          Limit the CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int           Limit the CPU CFS (Completely Fair Scheduler) quota
  -c, --cpu-shares int          CPU shares (relative weight)
      --cpuset-cpus string      CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string      MEMs in which to allow execution (0-3, 0,1)
      --disable-content-trust   Skip image verification (default true)
  -f, --file string             Name of the Dockerfile (Default is 'PATH/Dockerfile')
      --force-rm                Always remove intermediate containers
      --iidfile string          Write the image ID to the file
      --isolation string        Container isolation technology
      --label list              Set metadata for an image
  -m, --memory bytes            Memory limit
      --memory-swap bytes       Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --network string          Set the networking mode for the RUN instructions during build (default "default")
      --no-cache                Do not use cache when building the image
      --pull                    Always attempt to pull a newer version of the image
  -q, --quiet                   Suppress the build output and print image ID on success
      --rm                      Remove intermediate containers after a successful build (default true)
      --security-opt strings    Security options
      --shm-size bytes          Size of /dev/shm
  -t, --tag list                Name and optionally a tag in the 'name:tag' format
      --target string           Set the target build stage to build.
      --ulimit ulimit           Ulimit options (default [])

Package    Version
---------- -------
pip        22.0.4
setuptools 58.1.0
wheel      0.38.4
WARNING: You are using pip version 22.0.4; however, version 22.3.1 is available.
You should consider upgrading via the '/usr/local/bin/python -m pip install --upgrade pip' command.

--------------- ANSWER =  20530

with first_cte as (select *,
TO_CHAR(lpep_pickup_datetime, 'DD/MM/YYYY') as pickup_date,
TO_CHAR(lpep_dropoff_datetime, 'DD/MM/YYYY') as drop_date
from public.green_taxi_data)


select count(*) from first_cte 
where pickup_date = '15/01/2019' and drop_date = '15/01/2019'


---------------------- ANSWER = 15.01.2019

with first_cte as (select *,
TO_CHAR(lpep_pickup_datetime, 'DD/MM/YYYY') as pickup_date,
TO_CHAR(lpep_dropoff_datetime, 'DD/MM/YYYY') as drop_date
from public.green_taxi_data),
max_val as (
select max(trip_distance) as max_value from first_cte)

select pickup_date, drop_date from first_cte
where trip_distance = (select max_value from max_val )

-------------------------

with first_cte as (
select *,
TO_CHAR(lpep_pickup_datetime, 'DD/MM/YYYY') as pickup_date,
TO_CHAR(lpep_dropoff_datetime, 'DD/MM/YYYY') as drop_date
from public.green_taxi_data)


select passenger_count, count(passenger_count)
from first_cte
where pickup_date = '01/01/2019' and drop_date '01/01/2019'
group by passenger_count


--------------------------------------------------------  ANSWER = LONG ISLAND ect 



with first_cte as (
select *,
TO_CHAR(lpep_pickup_datetime, 'DD/MM/YYYY') as pickup_date,
TO_CHAR(lpep_dropoff_datetime, 'DD/MM/YYYY') as drop_date
from public.green_taxi_data),
second_cte as (
select first_cte.*, zones."Zone"
from first_cte 
inner join public.zones 
on first_cte."PULocationID" = zones."LocationID"
),
astoria_cte as (

select * from second_cte
where "Zone" = 'Astoria'),
drop_cte as (
select "DOLocationID", max(tip_amount) as max_tip from astoria_cte 
group by "DOLocationID"
order by max_tip desc)

select * from drop_cte 
left join zones 
on drop_cte."DOLocationID" = zones."LocationID"
order by max_tip desc
