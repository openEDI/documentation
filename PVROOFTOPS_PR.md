# PV Rooftop Database - Puerto Rico (PVRDB-PR)


## Description

The National Renewable Energy Laboratory's (NREL) PV Rooftop Database for Puerto Rico (PVRDB-PR) is a lidar-derived, geospatially-resolved dataset of suitable roof surfaces and their PV technical potential for virtually all buildings in Puerto Rico. The source lidar data were obtained from 2017 3cm NASA G-LiGHT and 2015-2016 <0.35m USGS 3DEP data. The building footprints were obtained from Open Street Map Buildings and were collected in 2018. Using GIS methods, NREL identified suitable roof surfaces based on their size, orientation, and shading parameters ([Gagnon et al. 2016](https://www.nrel.gov/docs/fy16osti/65298.pdf)). Standard 2020 technical potential was then estimated for each plane using the NREL [System Advisory Model's (SAM) PVWatts v5 model](https://developer.nrel.gov/docs/solar/pvwatts/v5/) and solar irradiance from the NREL [National Solar Radiation Database PSM3 for 2017](https://nsrdb.nrel.gov/).

The PVRDB-PR is downloadable by county for Puerto Rico. The data consists of PV suitable roof surfaces (aka "developable planes") for 96% of buildings in Puerto Rico. The developable planes contain orientation, area, spatial geometries, and technical potential details of each developable plane suitable for rooftop solar in Puerto Rico. 

The PV Rooftops dataset is provided in Parquet format partitioned by county name. The dataset can be queried directly via Athena or PrestoDB with relatively quick response time, or downloaded as a Parquet format data file from S3 (`s3://oedi-data-lake/pv-rooftop-pr/developable-planes`).


## Data Dictionary

Column | Type | Description
-- | -- | --
`devp_gid` | bigint | The developable plane geographic ID (**unique/primary-key**).
`bldg_plane_id` | integer | The plane ID unique to a given building (`bldg_fid`).
`bldg_fid` | integer | The building feature ID (**foreign key**). The `bldg_fid` can be tagged to the HOTOSM building `fid` column to obtain the building feature and geometry.
`bg_geoid` | text | The census block group GEOID. The `bg_geoid` is 12 characters long. The first 2 numbers represent the State FIPS, the next 3 are County FIPS, the next 6 are the Tract FIPS and the last 1 is the Block Group FIPS. 
`azimuth` | numeric | Plane azimuth angle (degrees). 0 values represent flat roofs.
`tilt` | numeric | The panel sloped tilt (degrees). For flat roofs (azimuth=0), we assume a 15-degree panel tilt. For tilted roofs, we use the tilt of the roof plane.
`flat_m2` | double precision | Flat "bird's-eye" area of the developable plane surface (meters-squared).
`slope_degrees` | numeric | Slope of the developable roof plane in degrees.
`slope_m2` | double precision | Slope area of the developable place surface (meters-squared).
`pitchmultiplier` | numeric | This is the multiplier value used to calculate the slope area from the flat roof area.
`pct_shading` | numeric | The average diffuse shading value (%) from 4 representative days (summer and winter solstices, autumnal and vernal equinoxes).
`area_derate_factor` | numeric | This is the area derate factor used to identify the PV module area from the total developable plane area. Options include 0.70 for flat roofs and 0.98 for tilted roofs. 
`dc_ac_ratio` | numeric | The DC to AC ratio. We used the PVWatts default value of 1.2.
`array_type` | integer | The PVWatts array type. We use Fixed-Tilt Rooftop PV (value=1) for all simulations.
`losses` | numeric | Total system losses (%). We used the PVWatts default of 14.08% for all simulations.
`module_type` | integer | The PV module type used in the PVWatts estimation. We used the "Standard" module type (value=0) for all simulations.
`inverter_efficiency` | integer | Inverter efficiency at rated power (%). We used the SAM PVWatts default of 96% for all simulations.
`annual_kwh` | double precision | Annual generation (kWh) potential of each array.
`the_geom_4326` | USER-DEFINED | Geometry in WGS84 (SRID=4326)
`the_geom_32620` | USER-DEFINED | Projected Geometry (SRID=32620)
`county` | text | Census county name

### Azimuth Coded Values

The azimuth values in PVRDB-PR are coded values which represent the median value of a range of azimuth degrees. The table below decodes the azimuth values with the value range and the ordinal direction.

Azimuth | Azimuth Value Range | Ordinal Direction
-- | -- | --
0 | Flat | Flat
45 | 22.5-67.5 | NE
90 | 67.5-112.5 | E
135 | 112.5-157.5 | SE
180 | 157.5-202.5 | S
225 | 202.5-247.5 | SW
270 | 247.5-292.5 | W
315 | 292.5-237.5 | NW
360 | 337.5-360; 0-22.5 | N

## Methods and Assumptions

The PVRDB data uses similar GIS methods as [PVRDB](https://registry.opendata.aws/nrel-oedi-pv-rooftops/) as described in [Gagnon et al. (2016)](https://www.nrel.gov/docs/fy16osti/65298.pdf). Unlike previous data versions, PVRDB-PR technical potential estimates are based on NREL’S PV Rooftop Model v2.0 updated assumptions, such as:
- Power Density is assumed to be 182 W/m2 (compared to the 160 W/m2 assumption in the 2016 U.S. study).
- North facing planes are not excluded for PR, however, they were excluded from the 2016 U.S. study. In PR, 13% of the rooftop PV technical potential is a N, NE, NW facing plane.
- Minimum size requirement for a developable plane is set to 1.62 m2, which is the average size required for 1 250-Watt solar panel. A building is considered suitable if it meets the other criteria and it has at least one plane large enough for a solar panel. In the 2016 U.S. Study, a suitable building needed a minimum 10m2. If we applied the same >= 10m2 assumption to Puerto Rico, generation would be reduced by ~4 TWh for the total building potential (all residential buildings). 
- The shading assumption in this PR assessment was updated to apply percent shading directly at the developable plane level into the System Advisor Model (SAM) when calculating generation potential. For the U.S. assessment, % shading was used to screen potential planes, but it was not used directly at the plane level when processed in SAM to get the generation; instead, the SAM default of 3% was applied. This new approach is more accurate than previous estimates, but it results in a lower kWh/kW estimate for Puerto Rico compared to the U.S.

### Data Sources

1. [NASA G-LiHT](https://gliht.gsfc.nasa.gov/index.php?section=49): 3cm LiDAR data collected in spring of 2017. This data only has partial PR island coverage.
2. [USGS 3DEP LiDAR](https://registry.opendata.aws/usgs-lidar/): <0.35 m nominal resolution LiDAR data collected in 2015 as part of the 2016 Commonwealth of Puerto Rico Project Lidar survey (UUID: `{C2C7A2AF-8228-4C10-8756-BA971DD63953}`). This data was used to fill in LiDAR coverage after G-LiHT data.
3. [OpenStreetMap HOT export of building footprints](https://data.humdata.org/dataset/hotosm_pri_buildings): builing footprint data used to identify buildings from the LiDAR data. HOTOSM export on 10/01/2018.

### Assumptions for Building Suitability

Requirement | Description
-- | --
Shading | Measured shading for four seasons and required an average of 80% unshaded surface
Azimuth | All possible azimuths
Tilt | Average surface tilt <= 60 degrees
Minimum Area | >= 1.62 m2 (area required for a single solar panel)

### Assumptions for PV Performance Simulations

PV System Characteristics | Value for Flat Roofs | Value for Tilted Roofs
-- | -- | --
Tilt | 15 degrees | Tilt of plane
Ratio of module area to suitable roof area | 0.7 | 0.98
Azimuth | 180 degrees (south facing) | Midpoint of azimuth class
Module Power Density | 183 W/m2
Total system losses | Varies (SAM defaults + individual surface % shading)
Inverter efficiency | 96%
DC-to-AC ratio | 1.2

## Python Connection Examples

Athena data connection using PyAthena:
```python

import pandas as pd
from pyathena import connect

conn = connect(
    s3_staging_dir='s3://<user-defined>/tracking-the-sun', ##user defined staging directory
    region_name='us-west-2',
    work_group='<USER SPECIFIC WORKGROUP>'  ##specify workgroup if exists
)
```

Example #1: Querying with a limit:
```python
df = pd.read_sql("SELECT * FROM oedi.pvrdb_pr_developable_planes limit 8;", conn)
```

Example #2: Querying for a specific county name:
```python
df = pd.read_sql("SELECT * FROM oedi.pvrdb_pr_developable_planes WHERE county = 'San Juan' limit 8;", conn)
```

Example #3: Querying for a specific county FIPS (FIPS=127):
```python
df = pd.read_sql("SELECT * FROM oedi.pvrdb_pr_developable_planes WHERE geoid LIKE '72127%' limit 8;", conn)
```

For jupyter notebook example see our notebook which includes partitions and data dictionary:
[examples repository](https://github.com/openEDI/open-data-access-tools/tree/integration/examples)

## Related Links:

1. [Puerto Rico Solar-for-All: LMI PV Rooftop Technical Potential and Solar Savings Potential](https://data.nrel.gov/submissions/144)
2. [Solar-For-All Interactive Web Map](https://maps.nrel.gov/solar-for-all/)
3. [PVRDB](https://registry.opendata.aws/nrel-oedi-pv-rooftops/)
4. [Rooftop Solar Photovoltaic Technical Potential in the United States: A Detailed Assessment](https://www.nrel.gov/docs/fy16osti/65298.pdf)
5. [Using GIS-based methods and lidar data to estimate rooftop solar technical potential in US cities](https://iopscience.iop.org/article/10.1088/1748-9326/aa7225/pdf)
6. [Estimating rooftop solar technical potential across the US using a combination of GIS-based methods, lidar data, and statistical modeling](https://iopscience.iop.org/article/10.1088/1748-9326/aaa554/pdf)
7. [Rooftop Photovoltaic Technical Potential in the United States](https://data.nrel.gov/submissions/121)
8. [U.S. PV-Suitable Rooftop Resources](https://data.nrel.gov/submissions/47)
9. [Rooftop Solar Technical Potential for Low-to-Moderate Income Households in the United States](https://www.nrel.gov/docs/fy18osti/70901.pdf)
10. [Rooftop Energy Potential of Low Income Communities in America REPLICA](https://data.nrel.gov/submissions/81)
11. [PVWattsV5 Documentation](https://pvwatts.nrel.gov/downloads/pvwattsv5.pdf)


## Disclaimer and Attribution

Copyright (c) 2020, Alliance for Sustainable Energy LLC, All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.