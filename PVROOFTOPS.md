# PV Rooftops

The PV Rooftops dataset is provided in Parquet format partitioned by city.  There are 4 core datasets stored in S3:

/aspects

  `gid` bigint, 
  `city` string, 
  `state` string, 
  `year` bigint, 
  `bldg_fid` bigint, 
  `aspect` bigint, 
  `the_geom_96703` string, 
  `the_geom_4326` string, 
  `region_id` bigint, 
  `__index_level_0__` bigint
  
/buildings

  `gid` bigint, 
  `bldg_fid` bigint, 
  `the_geom_96703` string, 
  `the_geom_4326` string, 
  `city` string, 
  `state` string, 
  `year` bigint, 
  `region_id` bigint, 
  `__index_level_0__` bigint
  
/developable_planes

  `bldg_fid` bigint, 
  `footprint_m2` double, 
  `slope` bigint, 
  `flatarea_m2` double, 
  `slopeconversion` double, 
  `slopearea_m2` double, 
  `zip` string, 
  `zip_perc` double, 
  `aspect` bigint, 
  `gid` bigint, 
  `city` string, 
  `state` string, 
  `year` bigint, 
  `region_id` bigint, 
  `the_geom_96703` string, 
  `the_geom_4326` string, 
  `__index_level_0__` bigint
  
/rasd

  `gid` bigint, 
  `the_geom_96703` string, 
  `the_geom_4326` string, 
  `city` string, 
  `state` string, 
  `year` bigint, 
  `region_id` bigint, 
  `serial_id` bigint, 
  `__index_level_0__` bigint



Within each core dataset there are paritions by city_state_year(YY) that can be queried directly via Athena or PrestoDB with relatively quick response times, or downloaded as a Parquet format data file. 

