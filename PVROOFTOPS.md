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

/developable_planes
/rasd

Within each core dataset there are paritions by city_state_year(YY) that can be queried directly via Athena or PrestoDB with relatively quick response times, or downloaded as a Parquet format data file. 

