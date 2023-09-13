## The Correlation Between Traffic Violations and Motor Vehicle Collisions in NYC ##




### CIS 4400 - CMWA ###




> * Muhammad Megahed - muhammad.megahed@baruchmail.cuny.edu (lead)
> * Cristopher Zuleta - cristopher.zuleta@baruchmail.cuny.edu
> * Steven Cai - steven.cai@baruchmail.cuny.edu
> * Joshua Chirinos - joshua.chirinos@baruchmail.cuny.edu 
> * Nazmus Sakib - nazmus.sakib@baruchmail.cuny.edu



#### Baruch College - Fall 2022 ####





Our group decided to find a correlation between the status of the city streets and the occurrence of vehicle collisions. For this we will utilize fields from the 311 database such as: Street Condition, Street Light Condition, Traffic Signal Condition.

For this we will integrate the daily database of Motor Vehicles Collisions - Crashes to establish a correlation and a trend in behaviors/complaints. For example, we will try to establish correlations between inebriated drivers and the severity of any traffic violations. The goal of this project is to find possible solutions, based on the nature of the complaint and collision
_____________________________________

### Milestone 1 - Datasets

Motor Vehicle Collisions - Crashes:
> https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95

311 Service Requests from 2010 to Present:
> https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9



***
### KPIs
>* Number of accidents per Zip Code.
>* Number of accidents per Street.
>* Number of complaints per Borough.
>* Number of fatal injuries
>* Number of non-fatal injuries
>* Number of fatal collisions per factor
>* Number of non-fatal collisions per factor
>* Number of accidents by contributing factor
>* Number of accidents by Latitude and Longitude
>* Number of accidents by hour of the day
>* Number of people injured by accident
>* Number of people killed by accident


***
### Milestone 2 - Initial-to-final ER Model
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/718acf24-134d-4628-ab7b-2bf77f5251dc)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/346f3ee8-e451-4a06-b7a3-66889da305f0)

***
### Milestone 3 - Updated Models
> 311 Model
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/26563ffe-96ad-43d0-bf80-d8a85e4d8f8a)
> Crash Model
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/5ca9a946-eeee-41a0-af54-ce6b9a8e7902)
> Combined Model
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/d5446164-887a-4e07-a735-9f2b01725c59)


***
### Milestone 4 - ETL Tool and Target DBMS
To implement our project, we will be using Google BigQuery and for our ETL tool we will be using DBT. We came to this decision after a group discussion and agreed that all members feel comfortable and capable of contributing to the project while working on this platform.

***
### Milestone 5 - ETL Programming
Data Profiling using Python for the 311_data and collisions_data between 2019 and 2022:

> Had an error with the 311 data but upon checking the report file, everything seemed to look good:
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/a8ee0366-a056-45b1-9dd5-aa55365b1106)


- Links to the full report in html file (need downloading for proper viewing)
> * __[311 Report File](https://drive.google.com/file/d/11GfXChnohXoN-72otq-wBypY5cEem9kp/view?usp=sharing)__
> * __[Collision Report File](https://drive.google.com/file/d/1oYmcjU9Oi7UqQ8RZ32VYrrXxOGnJ54GP/view?usp=sharing)__

***
#### For 311 Data:
Alerts varied between High Cardinality, Constant, High correlation, Missing, Unsupported, and one alert of Unique.
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/941e1198-74bf-46e2-92a6-8c2f8ca99b49)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/1740a6a4-7042-43f7-b8b6-06fe41f2490c)

***
#### For Collisions Data:
Alerts varied between High Cardinality, High correlation, Missing, Uniform, Zeros, and one alert of Unique
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/b1af853f-a163-4970-997e-f283467970f0)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/d693a357-6c57-4ee2-abb8-52b60f5e1764)

***
### In DBT:
* Created 2 folders within the models’ folder:
    * core: for the data and the fact tables.
    * Staging: for the dimensions
##### 
* Created 3 folders within the staging folder:
    * 311: for the 311 dimensions
    * collisions: for the collision dimensions
    * shared_models: for the shared dimensions (date, time, location)
 
***
#### Core:
* 311_data.sql query and fixing
```
with all_data as (
  select 
    unique_key,
    PARSE_DATETIME("%m/%d/%Y %I:%M:%S %p", TRIM(created_date)) AS created_date,
    PARSE_DATETIME("%m/%d/%Y %I:%M:%S %p", TRIM(closed_date)) AS closed_date,
    agency,
    agency_name,
    complaint_type,
    descriptor,
    location_type,
    incident_zip,
    incident_address,
    street_name,
    cross_street_1,
    cross_street_2,
    intersection_street_1,
    intersection_street_2,
    address_type,
    city,
    landmark,
    facility_type,
    status,
    PARSE_DATETIME("%m/%d/%Y %I:%M:%S %p", TRIM(due_date)) AS due_date,
    resolution_description,
    PARSE_DATETIME("%m/%d/%Y %I:%M:%S %p", TRIM(resolution_action_updated_date)) AS resolution_action_updated_date,
    community_board,
    bbl,
    borough,
    x_coordinate,
    y_coordinate,
    open_data_channel_type,
    park_facility_name,
    park_borough,
    vehicle_type,
    taxi_company_borough,
    taxi_pick_up_location,
    bridge_highway_name,
    bridge_highway_direction,
    road_ramp,
    bridge_highway_segment,
    latitude,
    longitude,
    location
  from
      {{ source ('project4400team10', '311model')}}
)
select
  unique_key,
  cast(FORMAT_DATE("%Y-%m-%d", created_date) as date) as created_date,
  format_timestamp("%H:%M:%S", created_date) as created_time,
  cast(FORMAT_DATE("%Y-%m-%d", closed_date) as date) as closed_date,
  format_timestamp("%H:%M:%S", closed_date) as closed_time,
  coalesce(agency, "Not Available") as agency,
  coalesce(agency_name, "Not Available")  as agency_name,
  CAST(complaint_type AS string) as complaint_type,
  coalesce(descriptor, "Unknown") as descriptor,
  coalesce(location_type, "Not Available") as location_type,
  coalesce(incident_zip, "Not Available") as incident_zip,
  coalesce(incident_address, "Not Available") as incident_address,
  street_name,
  cross_street_1,
  cross_street_2,
  concat(coalesce(street_name, "Unknown"), " and ", coalesce(cross_street_1, "Unknown")) as intersecting_streets,
  intersection_street_1,
  intersection_street_2,
  address_type,
  coalesce(city, "Not Available") as city,
  landmark,
  facility_type,
  status,
  cast(FORMAT_DATE("%Y-%m-%d", due_date) as date) as due_date,
  format_timestamp("%H:%M:%S", due_date) as due_time,
  resolution_description,
  cast(FORMAT_DATE("%Y-%m-%d", resolution_action_updated_date) as date) as resolution_action_updated_date,
  format_timestamp("%H:%M:%S", resolution_action_updated_date) as resolution_action_updated_time,
  community_board,
  bbl,
  coalesce(borough, "Not Available") as borough,
  x_coordinate,
  y_coordinate,
  open_data_channel_type,
  park_facility_name,
  park_borough,
  vehicle_type,
  taxi_company_borough,
  taxi_pick_up_location,
  bridge_highway_name,
  bridge_highway_direction,
  road_ramp,
  bridge_highway_segment,
  coalesce(round(latitude, 6), 0.0) as latitude,
  coalesce(round(longitude, 6), 0.0) as longitude,
  location
from all_data
WHERE created_date BETWEEN '2019-07-01' AND '2022-11-28'
AND complaint_type = "Street Condition"
AND latitude IS NOT NULL
AND longitude IS NOT NULL
```
* Collisions_data.sql query
```
with corrected as (
  select
    crash_date,
    case
      when crash_time like '_:__' then concat('0', crash_time) 
      else crash_time
      end as crash_time,
    borough,
    zip_code,
    latitude,
    longitude,
    location,
    on_street_name,
    cross_street_name,
    concat(coalesce(on_street_name, "Unknown"), " and ", coalesce(cross_street_name, "Unknown")) as intersecting_streets,
    off_street_name,
    number_of_persons_injured,
    number_of_persons_killed,
    number_of_pedestrians_injured,
    number_of_pedestrians_killed,
    number_of_cyclist_injured,
    number_of_cyclist_killed,
    number_of_motorist_injured,
    number_of_motorist_killed,
    contributing_factor_vehicle_1,
    contributing_factor_vehicle_2,
    contributing_factor_vehicle_3,
    contributing_factor_vehicle_4,
    contributing_factor_vehicle_5,
    collision_id,
    vehicle_type_code_1,
    vehicle_type_code_2,
    vehicle_type_code_3,
    vehicle_type_code_4,
    vehicle_type_code_5
  from
    {{ source ('project4400team10', 'collisions')}}
  WHERE
    latitude != 0 
    AND latitude IS NOT NULL
    AND longitude != 0
    AND longitude IS NOT NULL
    AND borough IS NOT NULL
    AND zip_code IS NOT NULL
)
select
  crash_date,
  CAST(PARSE_TIME("%H:%M", crash_time) AS STRING) as crash_time,
  borough,
  cast(zip_code as STRING) as zip_code,
  round(latitude, 6) as latitude,
  round(longitude, 6) as longitude,
  location,
  on_street_name,
  cross_street_name,
  intersecting_streets,
  off_street_name,
  number_of_persons_injured,
  number_of_persons_killed,
  number_of_pedestrians_injured,
  number_of_pedestrians_killed,
  number_of_cyclist_injured,
  number_of_cyclist_killed,
  number_of_motorist_injured,
  number_of_motorist_killed,
  coalesce(CONTRIBUTING_FACTOR_VEHICLE_1, "Unspecified") as CONTRIBUTING_FACTOR_VEHICLE_1,
  coalesce(CONTRIBUTING_FACTOR_VEHICLE_2, "Unspecified") as CONTRIBUTING_FACTOR_VEHICLE_2,
  contributing_factor_vehicle_3,
  contributing_factor_vehicle_4,
  contributing_factor_vehicle_5,
  collision_id,
  coalesce(vehicle_type_code_1, "Unknown") as vehicle_type_code_1,
  coalesce(vehicle_type_code_2, "Unknown") as vehicle_type_code_2,
  vehicle_type_code_3,
  vehicle_type_code_4,
  vehicle_type_code_5
from
  corrected
WHERE crash_date BETWEEN '2019-07-01' AND '2022-11-28'
```
***
#### Staging:
#### **311 Models:**
* 311_location.sql
```
with location_dim as (
  select distinct
    location_type,
    incident_address,
    incident_zip,
    intersecting_streets,
    city,
    borough,
    latitude,
    longitude
  from
    {{ ref ('311_data')}}
)
select
  row_number() over () as location_dim_id,
  location_type,
  incident_address,
  incident_zip,
  intersecting_streets,
  city,
  borough,
  latitude,
  longitude
from
  location_dim
```

* agency_type.sql
```
with agency_dim as (
  select distinct
    agency,
    agency_name
  from
    {{ ref ('311_data')}}
)
select
  row_number() over () as agency_type_dim_id,
  agency,
  agency_name
from
  agency_dim
```

* channel_type.sql
```
with channel_dim as (
  select distinct
    open_data_channel_type as open_data_channel_type
  from
    {{ ref ('311_data')}}
)
select
  row_number() over () as channel_type_dim_id,
  open_data_channel_type,
from
  channel_dim
```

* complaint_type.sql
```
with complaint as (
    select distinct
        complaint_type,
        descriptor
    from {{ ref ('311_data')}}
)
select 
row_number() over () as complaint_type_dim_id,
complaint_type,
descriptor
from complaint
```

* status.sql
```
with status_dimension as (
  select distinct
    status as status
  from
    {{ ref ('311_data')}}
)
select
  row_number() over () as status_dim_id,
  status,
from
  status_dimension
```

***
#### **Collision Models:**
* collision.sql
```
with collision_dim as (
  select distinct
    vehicle_type_code_1,
    vehicle_type_code_2
  from
    {{ ref ('collisions_data')}}
)
select 
  row_number() over () as collision_dim_id,
  *
from
  collision_dim
```

* collision_location.sql
```
with location_dim as (
  select distinct
    intersecting_streets,
    borough,
    zip_code,
    latitude,
    longitude
  from
    {{ ref ('collisions_data')}}
)
select
  row_number() over () as location_dim_id,
  *
from 
  location_dim
```

* factors.sql
```
with factors_dim as (
  select distinct
    CONTRIBUTING_FACTOR_VEHICLE_1,
    CONTRIBUTING_FACTOR_VEHICLE_2
  from
    {{ ref ('collisions_data')}}
)
select
  row_number() over () as factors_dim_id,
  *
from 
  factors_dim
```

* fatalities.sql
```
with fatal_dim as (
  select distinct
    number_of_persons_killed,
    number_of_pedestrians_killed,
    number_of_cyclist_killed,
    number_of_motorist_killed
  from
    {{ ref ('collisions_data')}}
)
select
  row_number() over () as fatalities_dim_id,
  *
from
  fatal_dim
```

* injuries.sql
```
with injur_dim as (
  select distinct
    number_of_persons_injured,
    number_of_pedestrians_injured,
    number_of_cyclist_injured,
    number_of_motorist_injured
  from
    {{ ref ('collisions_data')}}
)
select
  row_number() over () as injuries_dim_id,
  *
from
  injur_dim
```

***
#### **Shared Models:**
* date_dimenstion.sql (day as the lowest grain using "generate_date_Array")
```
SELECT
  row_number() over () as date_dim_id,
  d as full_date,
  EXTRACT(YEAR FROM d) AS year,
  EXTRACT(MONTH FROM d) AS month,
  EXTRACT(DAY FROM d) AS day,
FROM (
  SELECT
    *
  FROM
    UNNEST(GENERATE_DATE_ARRAY('2016-10-01', '2022-11-28', INTERVAL 1 DAY)) AS d )
```

* time_dimension.sql (second as the lowest grain using "generate_timestamp_array")
```
SELECT
  row_number() over () as time_dim_id,
  format_timestamp("%H:%M:%S", t) as full_time,
  EXTRACT(HOUR FROM t) AS hour,
  EXTRACT(MINUTE FROM t) AS minute,
  EXTRACT(SECOND FROM t) AS second
FROM (
  SELECT
    *
  FROM
    UNNEST(GENERATE_TIMESTAMP_ARRAY('2010-01-01', '2010-01-02', INTERVAL 1 SECOND)) AS t )
```

* location_dimension.sql (generated from the two staging locations made earlier)
```
with complaint_location as (
    select *
    from {{ref ("311_location")}}
),

collision_location as (
    select *
    from {{ref ("collisions_location")}}
),

location_dimension as (
    select
        location_type,
        coalesce(complaint_location.intersecting_streets, collision_location.intersecting_streets, "Not Applicable") as intersecting_streets,
        coalesce(complaint_location.incident_zip, collision_location.zip_code) as zip_code,
        coalesce(complaint_location.borough, collision_location.borough) as borough,
        coalesce(complaint_location.latitude, collision_location.latitude) as latitude, 
        coalesce(complaint_location.longitude, collision_location.longitude) as longitude
    from
        complaint_location
        full join collision_location
        on complaint_location.latitude = collision_location.latitude
        and complaint_location.longitude = collision_location.longitude
)
select
    row_number() over () as location_dim_id,
    *
from
    location_dimension
```

***
#### **core (fact tables):**
* fct_311.sql with screenshot of preview.
```
{{ config(
    materialized='table'
)}}

with location as (
    select *
    from {{ ref ("location_dimension")}}
),

agency as (
    select *
    from {{ ref ("agency_type")}}
),

channel as (
    select *
    from {{ ref ("channel_type")}}
),

complaint as (
    select *
    from {{ ref ("311_data")}}
),

complaint_types as (
    select *
    from {{ ref ("complaint_type")}}
),

status as (
    select *
    from {{ ref ("status")}}
),

dates as (
    select *
    from {{ ref ("date_dimension")}}
),

times as (
    select *
    from {{ ref("time_dimension")}}
),

fact_table as (
    select
        complaint.unique_key,
        complaint_types.complaint_type_dim_id,
        agency.agency_type_dim_id,
        dates.date_dim_id,
        times.time_dim_id,
        location.location_dim_id,
        channel.channel_type_dim_id,
        status.status_dim_id
    from
        complaint
    left join agency on complaint.agency = agency.agency
    and complaint.agency_name = agency.agency_name
    left join complaint_types on complaint.complaint_type = complaint_types.complaint_type
    and complaint.descriptor = complaint_types.descriptor
    left join dates on complaint.created_date = dates.full_date
    left join times on complaint.created_time = times.full_time
    left join location on complaint.intersecting_streets = location.intersecting_streets
    and complaint.incident_zip = location.zip_code
    and complaint.latitude = location.latitude
    and complaint.longitude = location.longitude
    and complaint.location_type = location.location_type
    left join channel on complaint.open_data_channel_type = channel.open_data_channel_type
    left join status on complaint.status = status.status
)
select *
from fact_table
```
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/0036e427-2610-4af3-838c-584bc0cf5699)


* fct_collisions.sql with screenshot of preview
```
{{ config(
    materialized='table'
)}}

with collisions as (
    select *
    from {{ ref ("collisions_data")}}
),

collision as (
    select *
    from {{ ref ("collision")}}
),

location as (
    select *
    from {{ ref ("location_dimension")}}
),

factors as (
    select *
    from {{ ref ("factors")}}
),

fatalities as (
    select *
    from {{ ref ("fatalities")}}
),

injuries as (
    select *
    from {{ ref ("injuries")}}
),

dates as (
    select *
    from {{ ref ("date_dimension")}}
),

times as (
    select *
    from {{ ref ("time_dimension")}}
),

fact_table as (
    select
        collisions.collision_id,
        collision.collision_dim_id,
        location.location_dim_id,
        factors.factors_dim_id,
        fatalities.fatalities_dim_id,
        injuries.injuries_dim_id,
        dates.date_dim_id,
        times.time_dim_id
    from
        collisions
    left join collision on collisions.vehicle_type_code_1 = collision.vehicle_type_code_1
    and collisions.vehicle_type_code_2 = collision.vehicle_type_code_2
    left join location on collisions.intersecting_streets = location.intersecting_streets
    and collisions.zip_code = location.zip_code
    and collisions.latitude = location.latitude
    and collisions.longitude = location.longitude
    left join factors on collisions.CONTRIBUTING_FACTOR_VEHICLE_1 = factors.CONTRIBUTING_FACTOR_VEHICLE_1
    and collisions.CONTRIBUTING_FACTOR_VEHICLE_2 = factors.CONTRIBUTING_FACTOR_VEHICLE_2
    left join fatalities on collisions.number_of_persons_killed = fatalities.number_of_persons_killed
    and collisions.number_of_pedestrians_killed = fatalities.number_of_pedestrians_killed
    and collisions.number_of_cyclist_killed = fatalities.number_of_cyclist_killed
    and collisions.number_of_motorist_killed = fatalities.number_of_motorist_killed
    left join injuries on collisions.number_of_persons_injured = injuries.number_of_persons_injured
    and collisions.number_of_pedestrians_injured = injuries.number_of_pedestrians_injured
    and collisions.number_of_cyclist_injured = injuries.number_of_cyclist_injured
    and collisions.number_of_motorist_injured = injuries.number_of_motorist_injured
    left join dates on collisions.crash_date = dates.full_date
    left join times on collisions.crash_time = times.full_time
)

select *
from fact_table
```
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/9b4605a9-59e9-4d21-8679-5d88454c7af7)

***
#### **YML and MD files and their content:**
* sources.yml (destination and documentation for sources)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/e110f63c-8b96-4b01-a0e1-2e791b098d0e)

* core.yml (documentation for 311_data, collisions_data, and fact tables)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/0143cc5a-af1d-4c4d-8914-16d09af8a98b)

* 311.yml (documentation for 311 dimensions)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/faef37c7-09bf-441a-9c79-66708151aec3)

* 311.md (documentation for status column made using a table)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/493e2b7d-f9f4-4b4a-9b78-35268a78e477)

* collisions.yml (documentation for collisions dimenstions)
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/acfcd0e7-8492-4505-b81d-caaa13a0bf9f)

* dbt_project.yml
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/09059eef-d47c-438f-9fc6-7f374859f1c7)

***
#### **Directory overview:**
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/813d963b-cc4a-4854-b098-2da5ba36ba8e)

***
#### **DBT commands:**
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/24cdf192-5288-4ca6-81b0-50c1e37793c1)

***
#### **Final DAG:**
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/925d1b9e-bc71-4a62-b4c5-4c8f1a72abbe)

***
#### **BigQuery overview:**
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/d83b5737-7089-4069-a106-a1fab9f231c5)

***
#### **Final view of the dimensional model:**
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/2c804905-420f-42e4-b0b0-8c08a791ee76)
> * The 311 data had five staging models: status, complaint_type, agency_type, channel_type, and the 311 location dimension.
> * The collisions data had five staging models: factors, collisions, fatalities, injuries, and the collisions’ location dimension.
> * The 311 location dimension and the collisions’ location dimension were used to create the location dimension for the fact tables.
> * The date and time dimensions were created generically using SQL functions and linked to the fact table. The format for time and data were also matched with the ones in 311 data and the collisions data.

***
#### **BI Application:**
The BI and Data Visualization tool was used to create a dashboard application consisting of four sheets to help users navigate through the important aspects of the data easily and be informed on some of the important KPIs:


***
* Number of Injuries:
> This visualization was created using columns from the injuries dimension and the “full date” column in the date dimension. The type of injured personnel was used as a color filter.
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/3f5fe28b-7fda-45e3-a47c-df851d67204d)

* Number of Fatalities:
> This visualization was created using columns from the fatalities dimension and the “full date” column in the date dimension. The type of personnel involved was used as a color filter.
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/2c26d067-7bb7-4af6-b5f3-a7a6b3d1e555)

* Number of Collisions Per Zip code
> This was a direct visualization that we created using the zip code and the collision id column from the fact table. The collision Id had to be changed to a measure and used “count” the aggregate. Also, Borough was used as a color filter
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/ecfa6c16-e49f-4983-8558-85f3fbfc1cc7)

* Zip Code Map:
> This visualization was created using the longitude and latitude as measures, zip code as the details, and borough as a color filter. 
> We also created a search parameter for this sheet that takes any zip code as a value, and the parameter was also added to the dashboard application.
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/a2e98c4d-19d2-42b0-ace5-55f872eb231e)

* Dashboard:
> The dashboard consists of the four sheets above and uses the “zip code map” sheet as a filter. The “search zip code” parameter was also added to the dashboard
![image](https://github.com/MoeMegahed/ETL-Process-for-Data-Warehousing-DBT-and-BigQuery/assets/91993986/207198c2-8563-43d0-b9fe-3df6441248fc)
> __[Link to the Dashboard on Tableau Public](https://public.tableau.com/app/profile/muhammad.megahed/viz/BIApplicationforCollisions311Data-Team10/Dashboard1?publish=yes)__




















