#  Photovoltaic Field Array Time-Series (PVDAQ)

## Description

The Photovoltaic field array (PVDAQ) data is composed of time-series, raw performance data taken through a variety of sensors connected to a PV array. The data is typically taken at 15 minute averaged resolution, but can vary between systems. NREL source data is typically aggregated into the main database every 24 hours. Data is then processed to the NREL PVDAQ data lake on a monthly basis.

Some datasets have been acquired through previous research agreements with site owners, and with their permission, have now been made public. Those datasets are static and do not show any additional data increments. 

Our researchers utilize the data to monitor the durability of PV systems under a wide variety of conditions. Similar data within NREL archives also provides insites into experimental emerging technology systems. Addtionally, the data has proven useful in assisting in the development of data quality assurance software, and data analysis and machine learning tools.

All Data for PVDAQ and DOE Solar Data Prize is covered under the [DOI:10.25984/1846021](https://dx.doi.org/10.25984/1846021)

### 2023 Solar Data Prize

The American-Made Solar Data Bounty Prize was open to U.S.-based PV system owners and entities authorized to share data from PV systems. These owners were invited to submit at least five years of historical time series data at a minimum of 15-minute time resolution for one or two of their systems. Datasets collected through this prize are meant to assist commercial and academic research and development efforts seeking to improve the accuracy of PV system modeling, and thus lower the risk associated with developing and operating those assets.

The Data Prize entries were submitted in one of two categories: systems < 5 MW DC capacity, and those >5 MW DC capacity. The data from the submissions are available to the public for download as part of the PVDAQ Data repository. The following are the system IDs of the winners, in numerical order, not placement by award.

#### < 5 MW DC system IDs:

* **2105** - A 237 kW multi building roof top deployment with highly variable mount orientations in Hawaii 
* **2107** - A 893 kW Fixed ground-mount facility in a highly active agricultural area in California
* **9068** - A 4.7 MW Single-axis tracked facility in Colorado

#### > 5 MW DC system IDs:

* **7333** - A 257 MW Single-axis tracker facility in California. This dataset is at a very high time resolution of 10s for all channels.
* **9069** - A 38.7 MW Fixed ground-mount facility in Georgia

#### Details on the Prize Datasets

These datasets differ from the regular PVDAQ repository storage architecture (See below) where data is broken down by year, month, and day. In each of the prize repositories the available metadata, any support files, and the entire dataset as was submittied and curated is available. Some of these the datasets are broken down by sensor channel set type, and in others the data is labeled by sensor channel tag names or bundles.

**Note:** *Some of the prize datasets are extremely large and can have 10s of GBs of data. These could take a long time to download so please plan accordingly*

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

## Bulk Downloads from the OEDI site

The PVDAQ Access repository contains a small python program that can bundle all the daily data from a site  and download it onto your local system. If accessing the data for a Solar Data Prize site, some adjustment to the code would be necessary, since all the data sits within a single directory for each site.

[PVDAQ Access](https://github.com/NREL/pvdaq_access)

## Data Sources

[https://www.nrel.gov/pv/real-time-photovoltaic-solar-resource-testing.html](https://www.nrel.gov/pv/real-time-photovoltaic-solar-resource-testing.html)

[https://www.nrel.gov/pv/rdtools.html](https://www.nrel.gov/pv/rdtools.html)

[https://www.nrel.gov/docs/fy17osti/69131.pdf](https://www.nrel.gov/docs/fy17osti/69131.pdf)

### Other Data Sources

#### DuraMAT
A multi-institution consortium focused on discovery, development, de-risking, and enabling the commercialization of new materials and designs for PV modules.<br/>
[Main Site](https://www.duramat.org/) <br/>

* [Validation models for PV performance](https://datahub.duramat.org/dataset/data-for-validating-models-for-pv-module-performance/)
* Machine Learning training set for validation of [satellite imagery of PV Array sites](https://datahub.duramat.org/dataset/satellite-images-training-and-validation-set)
* Machine Learning training set for [Detection of Inverter Clipping - Real Data](https://datahub.duramat.org/dataset/inverter-clipping-ml-training-set-real-data)
* Machine Learning training set for [Detection of Inverter Clipping - Simulated Data](https://datahub.duramat.org/dataset/inverter-clipping-ml-training-set-simulated-data) 
* Machine learning training set for [ Detection of soiling cleaning events](https://datahub.duramat.org/dataset/automated-pv-systems-cleaning-and-detection)
* Example data of [Soiling signal in time-series data](https://datahub.duramat.org/dataset/pvdaq-time-series-with-soiling-signal)
* Spectral Irradiance Data Sets [Albuqueque](https://datahub.duramat.org/project/spectral-irradiance-data-and-resources)

### Addtional Resources

[https://www.nrel.gov/pv/real-time-photovoltaic-solar-resource-testing.html](https://www.nrel.gov/pv/real-time-photovoltaic-solar-resource-testing.html)

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
