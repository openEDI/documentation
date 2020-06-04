# PV Array

The data is a time-series of raw performance data taken through a variety of sensors connected to an array. The data is taken 15 sec averaged resolution and aggregated into the main data base every 24 hours. The data is automatically harvested and aggregated by loggers and various scripts over the course of the day. We utilize the data to monitor existing systems for durability under a wide variety of conditions often cross-comparing performance between our test systems and other in the field. The data is used to help us develop and validate analysis tools to be used on other PV fleet systems globally.


/garage_array  
/rsf_array   
/stf_array   
/windsite_array   

`measured_on` timestamp,   
`sensor_name` string,   
`value` double   

PARTITIONED BY  
`year` string,   
`month` string   


Within each core dataset there are partitions by y and month which can be queried directly via Athena or PrestoDB with relatively quick response times, or downloaded as a Parquet format data file.

/garage_array   
Sensor Name Lookup   

1	metered_ac_power   
2	inv1_ac_power   
3	meter1_ac_power   
4	inv1_dc_voltage   
5	inv3_ac_power   
6	meter3_ac_power   
7	inv1_dc_current   
8	inv3_dc_power   
9	inv2_ac_power   
10	String1Current_unshaded   
11	inv1_dc_power   
12	inv2_dc_voltage   
13	dc_power   
14	inv2_dc_power   
15	String12Current_shaded   
16	inv_total_ac_power   
17	String2Current_unshaded   
18	inv3_dc_voltage   
19	inv3_dc_current   
20	inv2_dc_current   
21	meter2_ac_power   
22	String11Current_shaded   

/rsf_array   

sensor_name   
1	inv1_dc_voltage   
2	inv1_temp   
3	wind_speed   
4	inv2_dc_power   
5	inv1_dc_current   
6	inv2_dc_current   
7	ac_power   
8	dc_power   
9	inv2_dc_voltage   
10	inv2_temp   
11	POA_irradiance   
12	ambient_temp   
13	inv1_ac_power_kW   
14	ac_meter_1_power_kW   
15	module_temp   
16	ac_power_metered_kW   
17	inv1_dc_power   
18	POA_irradiance_refcell   
19	refcell_temp   
20	ac_meter_2_power_kW   
21	inv2_ac_power_kW   


/stf_array   

1	dc_voltage   
2	poa_irradiance   
3	PR   
4	module_temp   
5	ac_power   
6	ambient_temp   
7	kWh_net   

/windsite_array   
1	kWh_gross   
2	poa_irradiance   
3	ac_power   
4	PR   
5	ambient_temp   
6	dc_voltage   
7	kWh_net   
8	dc_current   
9	module_temp   
10	dc_power   
