# Running queries on ComStock using AWS Athena

ComStock data can be accessed and downloaded using [AWS Athena](https://aws.amazon.com/athena/).
Here we will show how to query the data for multiple building types, US regions, and upgrades.

## Requirements

An AWS account is necessary to run the code in this tutorial.

## Setting up AWS Athena with ComStock data for querying

First of all, we need to log into AWS and go to [AWS Athena Console](https://console.aws.amazon.com/athena/home).

NOTE: To run these queries, go to the `Query Editor` tab of the Athena interface, copy and paste the query into the `New query` box, and click the `Run query` button.

### Setting up the database
As a first step, we will create a database which the data we want to query will live in - if you already have a database you want to create the tables in, or are fine with the tables being created in a default database, skip this step.

```sql
CREATE DATABASE comstock
```

### Creating the metadata table
Next we need to create the data tables that will be used to query the data.
We will create two tables, one with building characteristics and annual energy data, and another with the time series energy data.

First we will create the metadata table, which will contain building characteristics and annual energy data. This is query should take less than 1 minute to run.

In the SQL in the Athena `Query Editor` tab:

```sql
CREATE EXTERNAL TABLE `comstock_v1_metadata`(
  `applicability` boolean,
  `bldg_id` bigint,
  `climate_zone` string,
  `in.aspect_ratio` double,
  `in.building_type` string,
  `in.climate_zone` string,
  `in.code_when_built` string,
  `in.cooling_fuel` string,
  `in.current_envelope_code` string,
  `in.current_exterior_lighting_code` string,
  `in.current_hvac_code` string,
  `in.current_interior_equipment_code` string,
  `in.current_interior_lighting_code` string,
  `in.floor_height` double,
  `in.heating_fuel` string,
  `in.hvac_delivery_type` string,
  `in.hvac_system_type` string,
  `in.number_of_stories` double,
  `in.rotation` double,
  `in.sqft` double,
  `in.water_systems_fuel` string,
  `in.weather_station` string,
  `in.weekday_opening_time` string,
  `in.weekday_operating_hours` string,
  `in.weekend_opening_time` string,
  `in.weekend_operating_hours` string,
  `out.electricity.cooling.energy_consumption` double,
  `out.electricity.cooling.energy_consumption_intensity` double,
  `out.electricity.cooling.energy_savings` double,
  `out.electricity.cooling.energy_savings_intensity` double,
  `out.electricity.exterior_lighting.energy_consumption` double,
  `out.electricity.exterior_lighting.energy_consumption_intensity` double,
  `out.electricity.exterior_lighting.energy_savings` double,
  `out.electricity.exterior_lighting.energy_savings_intensity` double,
  `out.electricity.fans.energy_consumption` double,
  `out.electricity.fans.energy_consumption_intensity` double,
  `out.electricity.fans.energy_savings` double,
  `out.electricity.fans.energy_savings_intensity` double,
  `out.electricity.heat_recovery.energy_consumption` double,
  `out.electricity.heat_recovery.energy_consumption_intensity` double,
  `out.electricity.heat_recovery.energy_savings` double,
  `out.electricity.heat_recovery.energy_savings_intensity` double,
  `out.electricity.heat_rejection.energy_consumption` double,
  `out.electricity.heat_rejection.energy_consumption_intensity` double,
  `out.electricity.heat_rejection.energy_savings` double,
  `out.electricity.heat_rejection.energy_savings_intensity` double,
  `out.electricity.heating.energy_consumption` double,
  `out.electricity.heating.energy_consumption_intensity` double,
  `out.electricity.heating.energy_savings` double,
  `out.electricity.heating.energy_savings_intensity` double,
  `out.electricity.humidification.energy_consumption` double,
  `out.electricity.humidification.energy_consumption_intensity` double,
  `out.electricity.humidification.energy_savings` double,
  `out.electricity.humidification.energy_savings_intensity` double,
  `out.electricity.interior_equipment.energy_consumption` double,
  `out.electricity.interior_equipment.energy_consumption_intensity` double,
  `out.electricity.interior_equipment.energy_savings` double,
  `out.electricity.interior_equipment.energy_savings_intensity` double,
  `out.electricity.interior_lighting.energy_consumption` double,
  `out.electricity.interior_lighting.energy_consumption_intensity` double,
  `out.electricity.interior_lighting.energy_savings` double,
  `out.electricity.interior_lighting.energy_savings_intensity` double,
  `out.electricity.peak_demand.energy_consumption` double,
  `out.electricity.peak_demand.energy_consumption_intensity` double,
  `out.electricity.peak_demand.energy_savings` double,
  `out.electricity.peak_demand.energy_savings_intensity` double,
  `out.electricity.pumps.energy_consumption` double,
  `out.electricity.pumps.energy_consumption_intensity` double,
  `out.electricity.pumps.energy_savings` double,
  `out.electricity.pumps.energy_savings_intensity` double,
  `out.electricity.refrigeration.energy_consumption` double,
  `out.electricity.refrigeration.energy_consumption_intensity` double,
  `out.electricity.refrigeration.energy_savings` double,
  `out.electricity.refrigeration.energy_savings_intensity` double,
  `out.electricity.total.energy_consumption` double,
  `out.electricity.total.energy_consumption_intensity` double,
  `out.electricity.total.energy_savings` double,
  `out.electricity.total.energy_savings_intensity` double,
  `out.electricity.water_systems.energy_consumption` double,
  `out.electricity.water_systems.energy_consumption_intensity` double,
  `out.electricity.water_systems.energy_savings` double,
  `out.electricity.water_systems.energy_savings_intensity` double,
  `out.natural_gas.cooling.energy_consumption` double,
  `out.natural_gas.cooling.energy_consumption_intensity` double,
  `out.natural_gas.cooling.energy_savings` double,
  `out.natural_gas.cooling.energy_savings_intensity` double,
  `out.natural_gas.heating.energy_consumption` double,
  `out.natural_gas.heating.energy_consumption_intensity` double,
  `out.natural_gas.heating.energy_savings` double,
  `out.natural_gas.heating.energy_savings_intensity` double,
  `out.natural_gas.interior_equipment.energy_consumption` double,
  `out.natural_gas.interior_equipment.energy_consumption_intensity` double,
  `out.natural_gas.interior_equipment.energy_savings` double,
  `out.natural_gas.interior_equipment.energy_savings_intensity` double,
  `out.natural_gas.total.energy_consumption` double,
  `out.natural_gas.total.energy_consumption_intensity` double,
  `out.natural_gas.total.energy_savings` double,
  `out.natural_gas.total.energy_savings_intensity` double,
  `out.natural_gas.water_systems.energy_consumption` double,
  `out.natural_gas.water_systems.energy_consumption_intensity` double,
  `out.natural_gas.water_systems.energy_savings` double,
  `out.natural_gas.water_systems.energy_savings_intensity` double,
  `out.other_fuel.heating.energy_consumption` double,
  `out.other_fuel.heating.energy_consumption_intensity` double,
  `out.other_fuel.heating.energy_savings` double,
  `out.other_fuel.heating.energy_savings_intensity` double,
  `out.other_fuel.interior_equipment.energy_consumption` double,
  `out.other_fuel.interior_equipment.energy_consumption_intensity` double,
  `out.other_fuel.interior_equipment.energy_savings` double,
  `out.other_fuel.interior_equipment.energy_savings_intensity` double,
  `out.other_fuel.total.energy_consumption` double,
  `out.other_fuel.total.energy_consumption_intensity` double,
  `out.other_fuel.total.energy_savings` double,
  `out.other_fuel.total.energy_savings_intensity` double,
  `out.other_fuel.water_systems.energy_consumption` double,
  `out.other_fuel.water_systems.energy_consumption_intensity` double,
  `out.other_fuel.water_systems.energy_savings` double,
  `out.other_fuel.water_systems.energy_savings_intensity` double,
  `out.site_energy.total.energy_consumption` double,
  `out.site_energy.total.energy_consumption_intensity` double,
  `out.site_energy.total.energy_savings` double,
  `out.site_energy.total.energy_savings_intensity` double,
  `state` string,
  `upgrade` bigint,
  `weight` double,
  `metadata_index` bigint,
  `in.applicable` boolean,
  `__index_level_0__` bigint)
ROW FORMAT SERDE
  'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
WITH SERDEPROPERTIES (
  'parquet.column.index.access'='true')
STORED AS INPUTFORMAT
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'
OUTPUTFORMAT
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION
  's3://nrel-pds-building-stock/comstock/athena/2020/comstock_v1/metadata/'
TBLPROPERTIES (
  'CrawlerSchemaDeserializerVersion'='1.0',
  'CrawlerSchemaSerializerVersion'='1.0',
  'UPDATED_BY_CRAWLER'='vizstock_oedi_comstock_v1_metadata',
  'averageRecordSize'='170',
  'classification'='parquet',
  'compressionType'='none',
  'objectCount'='10',
  'recordCount'='30560553',
  'sizeKey'='5293679830',
  'typeOfData'='file')
```

The response from Athena when the command has successfully run should look like this:
![CreateMetadata](https://github.com/NREL/ComStock/blob/cbianchi/documentation/documentation/Screen%20Shot%202021-04-15%20at%208.44.36%20PM.png?raw=true)

### Creating the time series table

Next we will create the timeseries table. This query can take anywhere from 1 minute to approximately 14 hours to run, so we advise starting this before bed! The wide range depends on whether or not it has been run recently by someone else, in which case AWS appears to use a cache rather than redoing things.

```sql
CREATE EXTERNAL TABLE `comstock_v1_state`(
  `timestamp` timestamp,
  `bldg_id` bigint,
  `out.electricity.cooling.energy_consumption` double,
  `out.electricity.cooling.energy_consumption_intensity` double,
  `out.electricity.cooling.energy_savings` double,
  `out.electricity.cooling.energy_savings_intensity` double,
  `out.electricity.exterior_lighting.energy_consumption` double,
  `out.electricity.exterior_lighting.energy_consumption_intensity` double,
  `out.electricity.exterior_lighting.energy_savings` double,
  `out.electricity.exterior_lighting.energy_savings_intensity` double,
  `out.electricity.fans.energy_consumption` double,
  `out.electricity.fans.energy_consumption_intensity` double,
  `out.electricity.fans.energy_savings` double,
  `out.electricity.fans.energy_savings_intensity` double,
  `out.electricity.heat_recovery.energy_consumption` double,
  `out.electricity.heat_recovery.energy_consumption_intensity` double,
  `out.electricity.heat_recovery.energy_savings` double,
  `out.electricity.heat_recovery.energy_savings_intensity` double,
  `out.electricity.heat_rejection.energy_consumption` double,
  `out.electricity.heat_rejection.energy_consumption_intensity` double,
  `out.electricity.heat_rejection.energy_savings` double,
  `out.electricity.heat_rejection.energy_savings_intensity` double,
  `out.electricity.heating.energy_consumption` double,
  `out.electricity.heating.energy_consumption_intensity` double,
  `out.electricity.heating.energy_savings` double,
  `out.electricity.heating.energy_savings_intensity` double,
  `out.electricity.humidification.energy_consumption` double,
  `out.electricity.humidification.energy_consumption_intensity` double,
  `out.electricity.humidification.energy_savings` double,
  `out.electricity.humidification.energy_savings_intensity` double,
  `out.electricity.interior_equipment.energy_consumption` double,
  `out.electricity.interior_equipment.energy_consumption_intensity` double,
  `out.electricity.interior_equipment.energy_savings` double,
  `out.electricity.interior_equipment.energy_savings_intensity` double,
  `out.electricity.interior_lighting.energy_consumption` double,
  `out.electricity.interior_lighting.energy_consumption_intensity` double,
  `out.electricity.interior_lighting.energy_savings` double,
  `out.electricity.interior_lighting.energy_savings_intensity` double,
  `out.electricity.peak_demand.energy_consumption` double,
  `out.electricity.peak_demand.energy_consumption_intensity` double,
  `out.electricity.peak_demand.energy_savings` double,
  `out.electricity.peak_demand.energy_savings_intensity` double,
  `out.electricity.pumps.energy_consumption` double,
  `out.electricity.pumps.energy_consumption_intensity` double,
  `out.electricity.pumps.energy_savings` double,
  `out.electricity.pumps.energy_savings_intensity` double,
  `out.electricity.refrigeration.energy_consumption` double,
  `out.electricity.refrigeration.energy_consumption_intensity` double,
  `out.electricity.refrigeration.energy_savings` double,
  `out.electricity.refrigeration.energy_savings_intensity` double,
  `out.electricity.total.energy_consumption` double,
  `out.electricity.total.energy_consumption_intensity` double,
  `out.electricity.total.energy_savings` double,
  `out.electricity.total.energy_savings_intensity` double,
  `out.electricity.water_systems.energy_consumption` double,
  `out.electricity.water_systems.energy_consumption_intensity` double,
  `out.electricity.water_systems.energy_savings` double,
  `out.electricity.water_systems.energy_savings_intensity` double,
  `out.natural_gas.cooling.energy_consumption` double,
  `out.natural_gas.cooling.energy_consumption_intensity` double,
  `out.natural_gas.cooling.energy_savings` double,
  `out.natural_gas.cooling.energy_savings_intensity` double,
  `out.natural_gas.heating.energy_consumption` double,
  `out.natural_gas.heating.energy_consumption_intensity` double,
  `out.natural_gas.heating.energy_savings` double,
  `out.natural_gas.heating.energy_savings_intensity` double,
  `out.natural_gas.interior_equipment.energy_consumption` double,
  `out.natural_gas.interior_equipment.energy_consumption_intensity` double,
  `out.natural_gas.interior_equipment.energy_savings` double,
  `out.natural_gas.interior_equipment.energy_savings_intensity` double,
  `out.natural_gas.total.energy_consumption` double,
  `out.natural_gas.total.energy_consumption_intensity` double,
  `out.natural_gas.total.energy_savings` double,
  `out.natural_gas.total.energy_savings_intensity` double,
  `out.natural_gas.water_systems.energy_consumption` double,
  `out.natural_gas.water_systems.energy_consumption_intensity` double,
  `out.natural_gas.water_systems.energy_savings` double,
  `out.natural_gas.water_systems.energy_savings_intensity` double,
  `out.other_fuel.heating.energy_consumption` double,
  `out.other_fuel.heating.energy_consumption_intensity` double,
  `out.other_fuel.heating.energy_savings` double,
  `out.other_fuel.heating.energy_savings_intensity` double,
  `out.other_fuel.interior_equipment.energy_consumption` double,
  `out.other_fuel.interior_equipment.energy_consumption_intensity` double,
  `out.other_fuel.interior_equipment.energy_savings` double,
  `out.other_fuel.interior_equipment.energy_savings_intensity` double,
  `out.other_fuel.total.energy_consumption` double,
  `out.other_fuel.total.energy_consumption_intensity` double,
  `out.other_fuel.total.energy_savings` double,
  `out.other_fuel.total.energy_savings_intensity` double,
  `out.other_fuel.water_systems.energy_consumption` double,
  `out.other_fuel.water_systems.energy_consumption_intensity` double,
  `out.other_fuel.water_systems.energy_savings` double,
  `out.other_fuel.water_systems.energy_savings_intensity` double,
  `out.site_energy.total.energy_consumption` double,
  `out.site_energy.total.energy_consumption_intensity` double,
  `out.site_energy.total.energy_savings` double,
  `out.site_energy.total.energy_savings_intensity` double)
PARTITIONED BY (
  `upgrade` bigint,
  `state` string)
ROW FORMAT SERDE
  'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
WITH SERDEPROPERTIES (
  'parquet.column.index.access'='true')
STORED AS INPUTFORMAT
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'
OUTPUTFORMAT
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION
  's3://nrel-pds-building-stock/comstock/athena/2020/comstock_v1/state/'
TBLPROPERTIES (
  'CrawlerSchemaDeserializerVersion'='1.0',
  'CrawlerSchemaSerializerVersion'='1.0',
  'UPDATED_BY_CRAWLER'='vizstock_oedi_comstock_v1_state',
  'averageRecordSize'='11',
  'classification'='parquet',
  'compressionType'='none',
  'objectCount'='30499408',
  'recordCount'='1068697644480',
  'sizeKey'='107871145911082',
  'typeOfData'='file')
```

You will likely be logged out of AWS while waiting for this to complete. To check on if the query has completed click on
the **History** tab next in the Athena GUI and look for the query that begins `CREATE EXTERNAL TABLE comstock_v1_state(`
\- when the query updates from running to completed you will know you are ready to proceed to the next step.

![CreateTable](https://github.com/NREL/ComStock/blob/cbianchi/documentation/documentation/Screen%20Shot%202021-04-15%20at%2011.03.03%20AM.png?raw=true)

### Establishing partitions on the time series table

We use partitions to make our queries against the time series table quicker and more cost efficient. By splitting up the
table by `state` and `upgrade` average query time decreases by ~48X and cost by even more. This works by having Athena
mark which folders in S3 contain data relevant to each of the possible values for `state` or `upgrade`, and then only
access those folders when the relevant value is requested. Guaranteeing that the partitions are all correctly
instantiated one last query, which should take between one and four hours to complete:

```sql
MSCK REPAIR TABLE comstock_v1_state
```

Once again, you may need to use the **History** tab in the Athena GUI to check when this command completes.


## Running queries against ComStock

Now that the tables have been created, we can query the data through Athena.

### Core example query

Let's start with an example of a query that uses the metadata table and time series table to retrieve hourly data from a
subset of the building simulations. In this example we'll get all available end-uses for a Medium Office in Wisconsin
for the baseline case (no upgrade has been applied).

```sql
SELECT
  sum(
    "comstock_v1_state"."out.electricity.cooling.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.cooling.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.exterior_lighting.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.exterior_lighting.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.fans.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.fans.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.heat_recovery.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.heat_recovery.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.heat_rejection.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.heat_rejection.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.heating.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.heating.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.humidification.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.humidification.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.interior_equipment.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.interior_equipment.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.interior_lighting.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.interior_lighting.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.peak_demand.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.peak_demand.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.pumps.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.pumps.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.refrigeration.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.refrigeration.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.total.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.total.energy_consumption",
  sum(
    "comstock_v1_state"."out.electricity.water_systems.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.electricity.water_systems.energy_consumption",
  sum(
    "comstock_v1_state"."out.natural_gas.cooling.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.natural_gas.cooling.energy_consumption",
  sum(
    "comstock_v1_state"."out.natural_gas.heating.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.natural_gas.heating.energy_consumption",
  sum(
    "comstock_v1_state"."out.natural_gas.interior_equipment.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.natural_gas.interior_equipment.energy_consumption",
  sum(
    "comstock_v1_state"."out.natural_gas.total.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.natural_gas.total.energy_consumption",
  sum(
    "comstock_v1_state"."out.natural_gas.water_systems.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.natural_gas.water_systems.energy_consumption",
  sum(
    "comstock_v1_state"."out.site_energy.total.energy_consumption" * "comstock_v1_metadata"."weight"
  ) AS "out.site_energy.total.energy_consumption",
  "comstock_v1_metadata"."state" AS "location",
  "comstock_v1_state"."upgrade" AS "comstock_v1_state_upgrade",
  month(
    "comstock_v1_state"."timestamp"
  ) AS "month",
  day(
    "comstock_v1_state"."timestamp"
  ) AS "day",
  hour(
    "comstock_v1_state"."timestamp"
  ) AS "hour"
FROM
  "comstock_v1_state",
  "comstock_v1_metadata"
WHERE
  "comstock_v1_state"."bldg_id" = "comstock_v1_metadata"."bldg_id"
  AND "comstock_v1_state"."state" IN ('55')
  AND "comstock_v1_state"."upgrade" = 0
  AND "comstock_v1_metadata"."upgrade" = 0
  AND "comstock_v1_metadata"."in.building_type" = 'MediumOffice'
GROUP BY
  "comstock_v1_metadata"."state",
  "comstock_v1_state"."upgrade",
  month(
    "comstock_v1_state"."timestamp"
  ),
  day(
    "comstock_v1_state"."timestamp"
  ),
  hour(
    "comstock_v1_state"."timestamp"
  )
ORDER BY
  "comstock_v1_metadata"."state",
  "comstock_v1_state"."upgrade",
  month(
    "comstock_v1_state"."timestamp"
  ),
  day(
    "comstock_v1_state"."timestamp"
  ),
  hour(
    "comstock_v1_state"."timestamp"
  )

```

![QuerySimple](https://github.com/NREL/ComStock/blob/cbianchi/documentation/documentation/Screen%20Shot%202021-04-15%20at%208.55.34%20PM.png?raw=true)

To download the returned data as a CSV file click the file icon highlighted in red in the screenshot above.

Now let's demonstrate so ways of changing up the query:

### How to query data for other building types?

All we have to do for this is change the following line in the `WHERE` clause in the example query:

```sql
    AND "comstock_v1_metadata"."in.building_type" = 'MediumOffice'
```

Instead of 'MediumOffice' we can use one any of the following building types:

'MediumOffice', 'LargeOffice', 'SecondarySchool', 'Hospital','Outpatient'

### How to query other states?

All we have to do for this is change the following line in the `WHERE` clause in the example query:

```sql
    AND "comstock_v1_state"."state" IN ('55')
```

Instead of '55' (Wisconsin) we can use the FIPS code for another state, as listed here:
https://www.nrcs.usda.gov/wps/portal/nrcs/detail/?cid=nrcs143_013696

### How to query an upgrade?

All we have to do for this is change the following lines in the `WHERE` clause in the example query:

```sql
    AND "comstock_v1_state"."upgrade" = 0
    AND "comstock_v1_metadata"."upgrade" = 0
```

Instead of '0' we can use one of the following options:
```
0: Baseline,
1: Upgrade Roof Insulation (R-19)
2: Upgrade Roof Insulation (R-30)
3: Upgrade Wall Insulation (R-13)
4: Upgrade Wall Insulation (R-30)
7: Add Window Film
8: Add Cool Roof
9: Add EIFS Wall Insulation
10: Add Electrochromic Windows (BleachedGlass)
11: Add Electrochromic Windows (TintedGlass)
12: Kitchen Exhaust Fan DCV
13: Upgrade Boiler (AFUE-81)
14: Upgrade Boiler (AFUE-83)
15: Upgrade Boiler (AFUE-94)
17: Upgrade Chiller (efficient)
18: Add Demand Control Ventilation
19: Add Economizer
20: Upgrade Furnace (AFUE-81)
21: Upgrade Furnace (AFUE-92)
22: Upgrade Furnace (AFUE-98)
23: Add Heat Recovery
24: Upgrade Motors
25: Add PTAC Controls
26: Upgrade Packaged Terminal Heat Pump (Code)
27: Upgrade Packaged Terminal Heat Pump (Efficient)
28: Upgrade Packaged Terminal Heat Pump (Highly Efficient)
29: Upgrade Packaged Terminal Air Conditioner (Code)
30: Upgrade Packaged Terminal Air Conditioner (Efficient)
31: Upgrade Packaged Terminal Air Conditioner (Highly Efficient)
32: Add VFD To Pumps
33: Upgrade RTU Air Source Heat Pump (IEER-13.3)
34: Upgrade RTU Air Source Heat Pump (IEER-15.0)
35: Upgrade RTU Air Source Heat Pump (IEER-16.5)
36: Upgrade RTU DX Air Conditioner (IEER-14.0)
37: Upgrade RTU DX Air Conditioner (IEER-15.5)
38: Upgrade RTU DX Air Conditioner (IEER-17.0)
39: Upgrade Split System DX Air Conditioner (SEER 14)
40: Upgrade Split System DX Air Conditioner (SEER 16)
41: Upgrade Split System DX Air Conditioner (SEER 18)
42: Upgrade Split System DX Air Conditioner (SEER 20)
45: Add Advanced Hybrid RTUs
46: Upgrade Air Filters
47: Add Brushless DC Compressor Motors
48: Reset Chilled Water Supply Temperature
49: Close Outdoor Air Dampers During Unoccupied Hours
50: Add Cold Climate Heat Pumps
51: Upgrade Duct Routing
52: Add Exhaust Fan Interlock
53: Reset Hot Water Temperature
55: Add Predictive Thermostats
56: Reset Supply Air
57: Add Thermoelastic Heat Pumps
58: Add Variable Speed Cooling Tower
61: Adjust Thermostat Setpoints
62: Upgrade Compact Lights
63: Add Lighting Occupancy Controls
64: Add Daylighting Controls
65: Upgrade High Bay Lights
66: Upgrade Linear Lights
67: Upgrade Outdoor Lights
68: Upgrade Specialty Lights
69: PC Power Management (Screen Saver)
70: PC Power Management (Desktop)
71: PC Power Management (Display)
72: PC Virtualization (Laptop)
73: PC Virtualization (Thin Client)
74: Advanced Power Strips (Power Strips)
75: Advanced Power Strips (Controllable Power Outlets)
76: Upgrade SWH Electric Storage Water Heater (Eff=0.88)
77: Upgrade SWH Electric Storage Water Heater (Eff=0.93)
78: Upgrade to Instantaneous Gas Water Heater
79: Upgrade SWH Gas Storage Water Heater (Eff=0.67)
80: Upgrade SWH Gas Storage Water Heater (Eff=0.70)
81: Upgrade SWH Gas Storage Water Heater (Eff=0.82)
82: Upgrade to Heat Pump Water Heater
83: Add Floating Heat Pressure Control
84: Add Refrigerated Walk-In Doorway Protection (Strip Curtain)
85: Add Refrigerated Walk-In Doorway Protection (Automatic Door Closer)
86: Add Refrigerated Walk-In Doorway Protection (Automatic Door Closer and Strip Curtain)
87: Upgrade Refrigerated Walk-In Motor (PSC)
88: Upgrade Refrigerated Walk-In Motor (ECM)
```

### How to change time resolution?

1. For changing the resolution to 15 minutes, all we have to do is modifying the lines just before the `FROM` clause in the example query above as follows:

```sql
    month(
    "comstock_v1_state"."timestamp"
    ) AS "month",
    day(
    "comstock_v1_state"."timestamp"
    ) AS "day",
    hour(
    "comstock_v1_state"."timestamp"
    ) AS "hour"
    minute(
    "comstock_v1_state"."timestamp"
    ) AS "minute"
    FROM
```

And then in the `GROUP BY` clause:

```sql
    GROUP BY
    "comstock_v1_metadata"."state",
    "comstock_v1_state"."upgrade",
    month(
    "comstock_v1_state"."timestamp"
    ),
    day(
    "comstock_v1_state"."timestamp"
    ),
    hour(
    "comstock_v1_state"."timestamp"
    )
    minute(
    "comstock_v1_state"."timestamp"
    )
```
And finally in the `ORDER BY` clause:
```sql
    ORDER BY
    "comstock_v1_metadata"."state",
    "comstock_v1_state"."upgrade",
    month(
    "comstock_v1_state"."timestamp"
    ),
    day(
    "comstock_v1_state"."timestamp"
    ),
    hour(
    "comstock_v1_state"."timestamp"
    )
    minute(
    "comstock_v1_state"."timestamp"
    )
```

2. For changing the resolution to 1 day, all we have to do is modifying the lines just before the `FROM` clause in the example query above as follows:

```sql
    month(
    "comstock_v1_state"."timestamp"
    ) AS "month",
    day(
    "comstock_v1_state"."timestamp"
    ) AS "day"
    FROM
```

And then in the `GROUP BY` clause:

```sql
    GROUP BY
    "comstock_v1_metadata"."state",
    "comstock_v1_state"."upgrade",
    month(
    "comstock_v1_state"."timestamp"
    ),
    day(
    "comstock_v1_state"."timestamp"
    )
```
And finally in the `ORDER BY` clause:
```sql
    ORDER BY
    "comstock_v1_metadata"."state",
    "comstock_v1_state"."upgrade",
    month(
    "comstock_v1_state"."timestamp"
    ),
    day(
    "comstock_v1_state"."timestamp"
    )
```

3. For changing the resolution to 1 month, all we have to do is modifying the lines just before the `FROM` clause in the example query above as follows:

```sql
    month(
    "comstock_v1_state"."timestamp"
    ) AS "month"
    FROM
```

And then in the `GROUP BY` clause:

```sql
    GROUP BY
    "comstock_v1_metadata"."state",
    "comstock_v1_state"."upgrade",
    month(
    "comstock_v1_state"."timestamp"
    )
```
And finally in the `ORDER BY` clause:
```sql
    ORDER BY
    "comstock_v1_metadata"."state",
    "comstock_v1_state"."upgrade",
    month(
    "comstock_v1_state"."timestamp"
    )
```

### How to average data rather than sums?

All we have to do for this is change the modify part in everythome it appears in the `SELECT` clause:

```sql
    sum(
```

It has to be changed with:
```sql
    avg(
```


### How to preview the table to see all available columns?

All we have to do for this is change the whole the example query above with what follows:

```sql
    SELECT *
    FROM
      "comstock_v1_state",
      "comstock_v1_metadata"
    WHERE
      "comstock_v1_state"."bldg_id" = "comstock_v1_metadata"."bldg_id"
      AND "comstock_v1_state"."state" IN ('55')
      AND "comstock_v1_state"."upgrade" = 0
      AND "comstock_v1_metadata"."upgrade" = 0
      AND "comstock_v1_metadata"."in.building_type" = 'MediumOffice'
    limit 10
```


# From here on follow the example above:


In each case say which block of code we're doing the update in - i.e. the group / order by clause or the where clause

things to include:


- How to filter by another variable - i.e. sqft or hvac system type
- How to get all the allowable values for a metadata filter (using select unique(col) from comstock_metadata_v0)


DONE
- How to query multiple states / building types
- How to query an upgrade
- How to change resolution - give changes for 15 min, daily, and monthly
- How to get average instead of sum by altering the select clauses
- How to preview the table to see what columns are available using select * from blah limit 10
