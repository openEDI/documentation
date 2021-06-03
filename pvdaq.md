#  Photovoltaic Field Array Time-Series (PVDAQ) 

## Description

The Photovoltaic Field Array Time-Series (PVDAQ) data is a time-series of raw performance data taken through a variety of sensors connected to an array. The data is taken 15 sec averaged resolution and aggregated into the main data base every 24 hours. The data is automatically harvested and aggregated by loggers and various scripts over the course of the day. 

We utilize the data to monitor existing systems for durability under a wide variety of conditions often cross-comparing performance between our test systems and other in the field. The data is used to help us develop and validate analysis tools to be used on other PV fleet systems globally.

## Data Dictionary

The PVDAQ data is partitioned by system_id, year, month and day. Raw data is reported at 15 minute increments in ISO 8601 date and time. The timestamp is striped and data is averaged daily. An example file output is included here.   

Filename: system_1208__date_2012_03_11.csv

measured_on\  date_time\
ac_meter_1_power_kw__1017\ 
ac_meter_2_power_kw__1018\
ac_power_metered_1_2__1133\
ac_power_metered_kw__1016\
dc_power__1132\
inv1_ac_power_kw__1019\
inv1_dc_current__1021\
inv1_dc_power__1130\
inv1_dc_voltage__1020\
inv1_temp__1022\
inv2_ac_power_kw__1023\
inv2_dc_current__1025\
inv2_dc_power__1131\
inv2_dc_voltage__1024\
inv2_temp__1026\
system_id

Note: not every site or system_id will contain data for each attribute included in the data dictionary.  

## Metadata

The data is aggregated from multiple sensor systems. Metadata is exported from the main system as json, and converted into a json stcuture in Athena that can be joined by system_id, the metadata inlcudes:

```

"Site": {
		"site_id":"", 
		"public_name":"", 
		"location":"", 
		"latitude":"", 
		"longitude":"", 
		"elevation":"", 
		"climate_type":"", 
		"av_temp":"", 
		"av_pressure":""
		}, 
	"System": {
		"system_id":"", 
		"site_id":"", 
		"public_name":"", 
		"area":"", 
		"power":"", 
		"started_on":"", 
		"ended_on":"", 
		"comments":""
		}, 

	"Mount": {
		"Mount 0": {
			"mount_id":"", 
			"name":"", 
			"manufacturer":"", 
			"model":"", 
			"tracking":"", 
			"azimuth":"", 
			"tilt":""
			}
		}, 
	"Inverters": {
		"Inverter 0": {
			"inverter_id":"", 
			"name":"", 
			"manufacturer":"", 
			"model":"", 
			"type":"", 
			"quantity": "", 
			"serial_num": "", 
			"comments": "", 
			"time_interval": "", 
			"modules_per_string": "", 
			"num_strings": ""
			}
		}, 
	"Modules": {
		"Module 0": {
			"module_id": "", 
			"inverter_id": "", 
			"name": "", 
			"manufacturer": "", 
			"model": "", 
			"type": "", 
			"quantity": "", 
			"start_on": "", 
			"end_on": "", 
			"serial_num": "", 
			"reference_module": "", 
			"comments": ""
			}
		}, 
	"Meters": {}, 
	"Other Instruments": {}
	}
```

## Data Format

The PVDAQ Dataset is made available in CSV and Parquet format on AWS and is partitioned by `system_id`, `year`, `month`, `day` in AWS Glue and Athena. The schema may change across dataset years on S3.
 - `s3://oedi-data-lake/pvdaq/`

## Model, Methods or Assumptions

### Data Sources

[https://www.nrel.gov/pv/real-time-photovoltaic-solar-resource-testing.html](https://www.nrel.gov/pv/real-time-photovoltaic-solar-resource-testing.html)

[https://pvdata.duramat.org](https://pvdata.duramat.org)

[https://www.nrel.gov/pv/rdtools.html](https://www.nrel.gov/pv/rdtools.html)

[https://www.nrel.gov/docs/fy17osti/69131.pdf](https://www.nrel.gov/docs/fy17osti/69131.pdf)


## Python Connection Examples

Athena data connection using PyAthena:
```python

import pandas as pd
from pyathena import connect

conn = connect(
    s3_staging_dir='s3://<user-defined>/<>', ##user defined staging directory
    region_name='us-west-2',
    work_group='<USER SPECIFIC WORKGROUP>'  ##specify workgroup if exists
)
```

Example #1: Querying with a limit:
```python
df = pd.read_sql("SELECT * FROM oedi.<> limit 8;", conn)
```

For jupyter notebook example see our notebook which includes partitions and data dictionary:
[examples repository](https://github.com/openEDI/open-data-access-tools/tree/integration/examples)


## Disclaimer and Attribution

Copyright (c) 2020, Alliance for Sustainable Energy LLC, All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
