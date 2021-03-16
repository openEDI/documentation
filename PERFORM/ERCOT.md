# The Electric Reliability Council of Texas (ERCOT)

Time-coincident data for ERCOT consists of two year (2017, 2018) of actuals
and one year (2018) of probabilistic forecasts. This data is provided at
various spatial (i.e., site-level, zone-level, and system-level) and temporal
scales (i.e., day-ahead, intra-hour, and intra-hour).

Data is provided for 125 existing wind sites, 22 existing solar sites, 139
proposed wind sites, and 204 proposed solar sites in ERCOT. Existing wind and
solar plants and their meta data are collected from multiple data sources,
including the U.S. Wind Turbine Database, OpenEI, and the U.S. Energy
Information Administration. Proposed wind and solar sites and their meta data
are collected from ERCOT Interconnection Queue. These wind and solar sites are
grouped into eight load/weather zones and the whole system area based on the
ERCOT topology.

The following variables are provided for ERCOT:
- Meta data (coordinates, capacity, and other configuration data):
    - 125 actual wind sites
    - 139 proposed wind sites
    - 22 actual solar sites
    - 204 proposed solar sites
- Actual data (power [MW]):
    - Wind power (site-level, zone-level, and system-level)
    - Solar power (site-level, zone-level, and system-level)
    - Load (zone-level and system-level)
- Probabilistic forecasting data (power [MW]):
    - Wind power (site-level, zone-level, and system-level)
    - Solar power (site-level, zone-level, and system-level)
    - Load (zone-level and system-level)

## Python Examples

Example scripts to extract solar resource data using python are provided below:
### Wind actual
Wind meta data, time index (time stamps), and actual power time series are
provided at multiple spatial-levels (site, BA, Zone). Below is an example of
reading in BA-level data:

```python
import h5py
import pandas as pd
import s3fs

s3_path = 'S3://arpa-e-perform/ERCOT/2018/Wind/Actuals/BA_level/BA_wind_actuals_2018.h5'
fs = s3fs.S3FileSystem(anon=True)
with fs.open(s3_path, 'rb') as s3:
  with h5py.File(s3_path, 'r') as f:
    df_meta = pd.DataFrame(f['data/meta'][...])
    time_index = pd.Datetime_index(f['data/time-index'][...])
    wind_actuals = f['data/forecasts'][...]
```

### Solar actual
Solar meta data, time index (time stamps), and actual power time series are
provided at multiple spatial-levels (site, BA, Zone). Below is an example of
reading in BA-level data:

```python
import h5py
import pandas as pd
import s3fs

s3_path = 'S3://arpa-e-perform/ERCOT/2018/Solar/Actuals/BA_level/BA_solar_actuals_2018.h5'
fs = s3fs.S3FileSystem(anon=True)
with fs.open(s3_path, 'rb') as s3:
  with h5py.File(s3_path, 'r') as f:
    df_meta = pd.DataFrame(f['data/meta'][...])
    time_index = pd.Datetime_index(f['data/time-index'][...])
    solar_actuals = f['data/forecasts'][...]
```

### Wind intra-hour forecasts
Wind meta data, time index (issue time and forecast time), and forecasting
power time series [Actual, Deterministic Forecast, Percentile 1, ...,
Percentile 99] are provided at multiple spatial-levels (site, BA, Zone). Below
is an example of reading in BA-level data:

```python
import h5py
import pandas as pd
import s3fs

s3_path = 'S3://arpa-e-perform/ERCOT/2018/Wind/Forecasts/Intra-hour/BA_level/BA_wind_intra-hour_fcst_2018.h5'
fs = s3fs.S3FileSystem(anon=True)
with fs.open(s3_path, 'rb') as s3:
  with h5py.File(s3_path, 'r') as f:
    df_meta = pd.DataFrame(f['data/meta'][...])
    time_index = pd.Datetime_index(f['data/time-index'][...])
    wind_fcsts= f['data/forecasts'][...]
```

### Solar intra-hour forecasts
Solar meta data, time index (issue time and forecast time), and forecasting
power time series [Actual, Deterministic Forecast, Percentile 1, ...,
Percentile 99] are provided at multiple spatial-levels (site, BA, Zone). Below
is an example of reading in BA-level data:

```python
import h5py
import pandas as pd
import s3fs

s3_path = 'S3://arpa-e-perform/ERCOT/2018/Solar/Forecasts/Intra-hour/BA_level/BA_solar_intra-hour_fcst_2018.h5'
fs = s3fs.S3FileSystem(anon=True)
with fs.open(s3_path, 'rb') as s3:
  with h5py.File(s3_path, 'r') as f:
    df_meta = pd.DataFrame(f['data/meta'][...])
    time_index = pd.Datetime_index(f['data/time-index'][...])
    solar_fcsts = f['data/forecasts'][...]
```

### Load intra-hour forecasts
Load meta data, time index (issue time and forecast time), and forecasting
load time series [Actual, Percentile 1, ..., Percentile 99] are provided at
multiple spatial-levels (site, BA, Zone). Below is an example of reading in
BA-level data:

```python
import h5py
import pandas as pd
import s3fs

s3_path = 'S3://arpa-e-perform/ERCOT/2018/Load/Forecasts/Intra-hour/BA_level/BA_load_intra-hour_fcst_2018.h5'
fs = s3fs.S3FileSystem(anon=True)
with fs.open(s3_path, 'rb') as s3:
  with h5py.File(s3_path, 'r') as f:
    df_meta = pd.DataFrame(f['meta'][...])
    issue_time = pd.Datetime_index(f['issue_time'][...])
    horizon_time = pd.Datetime_index(f['issue_time'][...])
    load_actuals = f['actuals'][...]
    load_fcsts = f['forecast'][...]
```

## CHANGLOG
### Load Forecasts
- Fixed code which affected distribution of forecasts
- Added last Day-ahead issue time forecast
- Added Bias term to combining deterministic forecasts
- Optimized hyperparameters for combining deterministic forecasts with bias term
- Corrected Actual downsampling error.
- Adjusted error in BA Day-ahead timestamp.

### Day-ahead and intra-hour solar forecasts
- Fixed the issue time of day-ahead solar forecasts to UTC
- Updated distribution of forecasts
- Updated the zone-level forecasts
- Changed the dtype of time stamps to characters

### Day-ahead and intra-hour wind forecasts
- Fixed the issue time of day-ahead wind forecasts to UTC
- Updated distribution of forecasts
- Updated the zone-level forecasts
- Updated the BA-level forecasts
- Changed the dtype of time stamps to characters

### Meta data
- Solar meta data is cleaned and updated

### ECMWF-based deterministic power forecasts
- Added 51 members of ECMWF-based solar and wind day-ahead and intra-hour forecasts

### Change file naming convetion to:

#### Load Actuals
- BA_load_actuals_2018.h5
- Zone_Far_West_load_actuals_2018.h5

#### Solar Actuals
- BA_solar_actuals_2018.h5
- BA_solar_actuals_Existing_2018.h5
- Zone_Far_West_solar_actuals_2018.h5
- Zone_Far_West_solar_actuals_Existing_2018.h5
- Site_Noble_Solar_solar_actuals_2018.h5

#### Solar Actuals
- BA_wind_actuals_2018.h5
- BA_wind_actuals_Existing_2018.h5
- Zone_Far_West_wind_actuals_2018.h5
- Zone_Far_West_wind_actuals_Existing_2018.h5
- Site_Aguayo_Wind_wind_actuals_2018.h5

#### Load Forecasts
- BA_load_day-ahead_fcst_2018.h5
- BA_load_intra-day_fcst_2018.h5
- BA_load_intra-hour_fcst_2018.h5
- Zone_Far_West_load_day-ahead_fcst_2018.h5
- Zone_Far_West_load_intra-day_fcst_2018.h5
- Zone_Far_West_load_intra-hour_fcst_2018.h5

#### Solar Forecasts
- BA_solar_day-ahead_fcst_2018.h5
- BA_solar_intra-day_fcst_2018.h5
- BA_solar_intra-hour_fcst_2018.h5
- Zone_Far_West_solar_day-ahead_fcst_2018.h5
- Zone_Far_West_solar_intra-day_fcst_2018.h5
- Zone_Far_West_solar_intra-hour_fcst_2018.h5
- Site_Noble_Solar_solar_day-ahead_fcst_2018.h5
- Site_Noble_Solar_solar_intra-day_fcst_2018.h5
- Site_Noble_Solar_solar_intra-hour_fcst_2018.h5

#### Wind Forecasts
- BA_wind_day-ahead_fcst_2018.h5
- BA_wind_intra-day_fcst_2018.h5
- BA_wind_intra-hour_fcst_2018.h5
- Zone_Far_West_wind_day-ahead_fcst_2018.h5
- Zone_Far_West_wind_intra-day_fcst_2018.h5
- Zone_Far_West_wind_intra-hour_fcst_2018.h5
- Site_Aguayo_Wind_wind_day-ahead_fcst_2018.h5
- Site_Aguayo_Wind_wind_intra-day_fcst_2018.h5
- Site_Aguayo_Wind_wind_intra-hour_fcst_2018.h5
