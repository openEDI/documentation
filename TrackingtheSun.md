# LBNL Tracking the Sun

## Description

Berkeley Lab’s Tracking the Sun report series is dedicated to summarizing installed prices and other trends among grid-connected, distributed solar photovoltaic (PV) systems in the United States. The present report, the 11th edition in the series, focuses on systems installed through year-end 2017, with preliminary trends for the first half of 2018. As in years past, the primary emphasis is on describing changes in installed prices over time and variation in pricing across projects based on location, project ownership, system design, and other attributes. New to this year, however, is an expanded discussion of other project characteristics in the large underlying data sample. Future editions will include more of such material, beyond the report’s traditional focus on installed pricing.

The trends described in this report derive primarily from project-level data reported to state agencies and utilities that administer PV incentive programs, solar renewable energy credit (SREC) registration systems, or interconnection processes. In total, data were collected and cleaned for more than 1.3 million individual PV systems, representing 81% of U.S. residential and non-residential PV systems installed through 2017. The analysis of installed pricing trends is based on a subset of roughly 770,000 systems with available installed price data.

A technical summary of the dataset is as follows:

Focuses on projects installed through 2018 with preliminary data for the first half of 2019:
- Describes and analyzes trends related to Project characteristics, including system size and design, ownership, customer segmentation, and other attributes
- National median installed prices, both long-term and recent trends, focusing on host-owned systems
- Variability in pricing across projects according to system size, state, installer, module efficiency, inverter technology, residential new construction vs. retrofit, tax-exempt vs. commercial site hosts, and mounting configuration
- Distributed PV for the purpose of this report, includes residential and non-residential systems that are roof-mounted (of any size) or groundmounted up to 5 MWAC

Tracking the Sun relies on project-level data:
- Provided by state agencies and utilities that administer PV incentive programs, renewable energy credit registration (REC) systems, or interconnection processes
- Some of these data already exist in the public domain (e.g., California’s Currently Interconnected Dataset), though LBNL may receive supplementary fields, in some cases covered under non-disclosure agreements
- 67 entities spanning 30 states contributed data to this year’s report (Table A-1 in the Appendix of the report )

Customer Segments
- Residential: Single-family and, depending on the data provider, may also include multi-family
- Small Non-Residential: Non-residential systems ≤100 kWDC
- Large Non-Residential: Non-residential systems >100 kWDC (and ≤5,000 kWAC if ground-mounted) * Independent of whether connected to the customer- or utility-side of the meter

Units
- Real 2018 dollars
- Direct current (DC) Watts (W), unless otherwise noted

Installed Price: Up-front $/W price paid by the PV system owner, prior to incentives

Sample Frames and Data cleaning
Full sample: (Used to describe system characteristics, the basis for the public dataset)
1. Remove systems with missing size or install date
2. Standardize installer, module, inverter names
3. Integrate equipment spec sheet data
– Module efficiency and technology type
– Flag microinverters or DC optimizers
4. Convert dollar and kW values to appropriate units,
and compute other derived fields

Installed-Price Sample: (Used in analysis of installed prices)
5. Remove systems if:
– Missing installed price data
– Third-party owned (TPO)
– Battery storage included
– Self-installed

##Directory Structure

The Tracking the Sun Dataset is made available in Parquet format on AWS and is partitioned by `state` in AWS Glue and Athena. The schema may change across dataset years on S3.

 - `s3://lbnl-tracking-the-sun`

##python Connection examples

```python

import pandas as pd
from pyathena import connect

conn = connect(
    s3_staging_dir='s3://<user-defined>/tracking-the-sun', ##user defined staging directory
    region_name='us-west-2',
    ##work_group='<USER SPECIFIC WORKGROUP>'  specify workgroup if exists
)

df = pd.read_sql("SELECT * FROM oedi.oedi_tracking_the_sun_2019 limit 8;", conn)
```
For jupyter notebook example see our notebook which includes partitions and data dictionary:
[examples repository](https://github.com/openEDI/open-data-access-tools/tree/integration/examples)

##Data Dictionary for 2019 Dataset:

    Available States
    1. AR
    2. AZ
    3. CA
    4. CO
    5. CT
    6. DC
    7. DE
    8. FL
    9. IL
    10. KS
    11. MA
    12. MD
    13. ME
    14. MN
    15. MO
    16. NH
    17. NJ
    18. NM
    19. NY
    20. OH
    21. OR
    22. PA
    23. RI
    24. TX
    25. UT
    26. VT
    27. WI
    28. WA

  `data_provider`       	string\
  `system_id_from_first_data_provider`	string\
  `system_id_from_second_data_provider_if_applicable`	string\
  `system_id_tracking_the_sun`	string\
  `installation_date`   	date\
  `system_size`         	double\
  `total_installed_price`	double\
  `appraised_value_flag`	boolean\
  `sales_tax_cost`      	double\
  `rebate_or_grant`     	double\
  `performance_based_incentive_annual_payment`	double\
  `performance_based_incentives_duration`	int\
  `feed_in_tariff_annual_payment`	double\
  `feed_in_tariff_duration`	int\
  `customer_segment`    	string\
  `new_construction`    	int\
  `tracking`            	int\
  `ground_mounted`      	int\
  `battery_system`      	int\
  `zip_code`            	string\
  `city`                	string\
  `utility_service_territory`	string\
  `third_party_owned`   	int\
  `installer_name`      	string\
  `self_installed`      	int\
  `azimuth_1`           	double\
  `azimuth_2`           	double\
  `azimuth_3`           	double\
  `tilt_1`              	double\
  `tilt_2`              	double\
  `tilt_3`              	double\
  `module_manufacturer_1`	string\
  `module_model_1`      	string\
  `module_manufacturer_2`	string\
  `module_model_2`      	string\
  `module_manufacturer_3`	string\
  `module_model_3`      	string\
  `additional_module_model`	int\
  `module_technology_1` 	string\
  `module_technology_2` 	string\
  `module_technology_3` 	string\
  `bipv_module_1`       	int\
  `bipv_module_2`       	int\
  `bipv_module_3`       	int\
  `module_efficiency_1` 	double\
  `module_efficiency_2` 	double\
  `module_efficiency_3` 	double\
  `inverter_manufacturer_1`	string\
  `inverter_manufacturer_2`	string\
  `inverter_manufacturer_3`	string\
  `inverter_model_1`    	string\
  `inverter_model_2`    	string\
  `inverter_model_3`    	string\
  `microinverter_1`     	int\
  `microinverter_2`     	int\
  `microinverter_3`     	int\
  `system_inverter_capacity`	double\
  `dc_optimizer`        	int\
  `inverter_loading_ratio`	double\
  `state`               	string\

##References

[https://emp.lbl.gov/sites/default/files/tracking_the_sun_2018_edition_final_0.pdf](https://emp.lbl.gov/sites/default/files/tracking_the_sun_2018_edition_final_0.pdf)

[https://emp.lbl.gov/sites/default/files/tracking_the_sun_2018_briefing_0.pdf](https://emp.lbl.gov/sites/default/files/tracking_the_sun_2018_briefing_0.pdf)


## Disclaimer and Attribution

Copyright (c) 2020, Alliance for Sustainable Energy LLC, All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
