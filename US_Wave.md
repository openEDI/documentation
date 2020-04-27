# High Resolution Ocean Surface Wave Hindcast

## Description

The development of this dataset was funded by the U.S. Department of Energy, Office of Energy Efficiency & Renewable Energy, Water Power Technologies Office to improve our understanding of the U.S. wave energy resource and to provide critical information for wave energy project development and wave energy converter (WEC) conceptual design. This will be the highest resolution publicly available long-term wave hindcast dataset that – when complete – covers the entire U.S. EEZ. As such, it could be of value to any company with marine operations inside the U.S. EEZ. Specifically, the data can be used to investigate the historical record of wave statistics at any U.S. site. This level of detail could be of interest to the Oil and Gas industry for offshore platform engineering, to the offshore wind industry for turbine and array design, to offshore aquaculture production and blue economy development, to coastal communities for extreme hazards mitigation,  to global shipping companies and fisherman for a better understanding of weather windows and seasonal wave climate patterns at a spatial resolution that does not exist elsewhere. The NREL Offshore Wind group has expressed significant interest in this dataset for device structural modeling, array design, and economic modeling.

## Model

This is the highest resolution publicly available wave hindcast dataset. The multi-scale, unstructured-grid modeling approach using WaveWatch III and SWAN enabled long-term (decades) high-resolution hindcasts in a large regional domain. The model was extensively validated not only for the most common wave parameters, but also six IEC resource parameters and 2D spectra with high quality spectral data derived from publicly available buoys. This creation of this dataset was funded by the U.S. Department of Energy, Office of Energy Efficiency & Renewable Energy, Water Power Technologies Office under Contract DE-AC05-76RL01830 to Pacific Northwest National Laboratory (PNNL). Additional details on detailed definitions of the variables found in the dataset, the SWAN and WaveWatch III model configuration and model validation are available in a peer-review publication [Development and validation of a high-resolution regional wave hindcast model for U.S. West Coast wave resource characterization](https://www.osti.gov/biblio/1599105) and a PNNL technical report: [High-Resolution Regional Wave Hindcast for the U.S. West Coast](https://www.osti.gov/biblio/1573061/). This study was funded by the U.S. Department of Energy, Office of Energy Efficiency & Renewable Energy, Water Power Technologies Office under Contract DE-AC05-76RL01830 to Pacific Northwest National Laboratory (PNNL).

The following variables were extracted from the SWAM Model data:
  - Dir: Direction Normal to the Wave Crests
  - Hsig: Calculated as the zeroth spectral moment (i.e., H_m0)
  - Period: Resolved Spectral Moment (m_0/m_1)
  - RTpeak: The period associated with the maximum value of the wave energy spectrum
  - Tm02: Total wave energy flux from all directions
  - Tm_10: Spectral width characterizes the relative spreading of energy in the wave spectrum.
  - d: Fraction of total wave energy travelling in the direction of maximum wave power
  - djdmax: The direction from which the most wave energy is travelling
  - owp: Total wave energy flux from all directions
  - sw: Spectral width characterizes the relative spreading of energy in the wave spectrum. 

## Domains

The dataset currently covers the U.S. Exclusive Economic Zone (‘EEZ’, up to 200 nautical miles from shore) offshore of the West Coast, and includes shallow nearshore regions not covered by previous model hindcasts. Future additions to the dataset will extend the coverage to the entire U.S. EEZ, including Island territories. The dataset has a 3-hour timestep spanning 32 years from 1979 through 2010. It includes the most common wave statistics (wave height, wave direction, wave period), alongside several other wave statistics developed for the wave energy sector. The dataset was generated from the unstructured-grid  SWAN model output that was driven by a WaveWatch III model with global-regional nested grids. The SWAN model simulations were performed with a spatial resolution as fine as 200 meters in shallow waters:

- West Coast United States: Dataset Available
- East Coast United States: Available soon
- Alaskan Coast: Available soon

## References

Users of the High Resolution Ocean Surfae Wave Hindcast should use the following citations:
- [Wu, Wei-Cheng, et al. "Development and validation of a high-resolution regional wave hindcast model for US West Coast wave resource characterization." Renewable Energy 152 (2020): 736-753.](https://www.osti.gov/biblio/1599105)
- [Yang, Zhaoqing, et al. High-Resolution Regional Wave Hindcast for the US West Coast. No. PNNL-28107. Pacific Northwest National Lab.(PNNL), Richland, WA (United States), 2018.](https://www.osti.gov/biblio/1573061/)

## Directory structure

High Resolution Ocean Surface Wave Hindcast data is made available as a series of hourly .h5 files
corresponding to each domain and year. Below is an example of the directory
structure for the West Coast US EEZ domain:
- s3://nrel-pds-wtk/hdf5-source-files-hourly/conus -> root directory for the conus domain
    - /v1.0.0 -> version 1 of the data corresponding to years 2007-2013, run by 3Tier
        - /wtk_conus_${year}.h5 -> Hourly data for all variables for the given year
        - /${year}/wind_${hub_height}.h5 -> Five minute wind resource data for the given year and hub height
    - /v1.1.0 -> version 1.1 of the data corresponding to 2014, run under NARIS with an updated version of WRF and new Boundary Layer Physics (PBL scheme)

The WIND Toolkit data is also available via HSDS at /nrel/wtk/${domain} where
domain is conus, canada, or mexico

For examples on setting up and using HSDS please see our [examples repository](https://github.com/nrel/hsds-examples)

## Data Format

The data is provided in high density data file (.h5) separated by year. The
variables mentioned above are provided in 2 dimensional time-series arrays with
dimensions (time x location). The temporal axis is defined by the `time_index`
dataset, while the positional axis is defined by the `coordinate` dataset. The units for the
variable data is also provided as an attribute (`units`). The SWAN and IEC valiable names are also provide under the attributes ('SWAWN_name') and ('IEC_name') respectively.

## Python Examples

Example scripts to extract wind resource data using python are provided below:

The easiest way to access and extract data from the Resource eXtraction tool
[`rex`](https://github.com/nrel/rex)


```python
from rex import WindX

wtk_file = '/nrel/wtk/conus/wtk_conus_2010.h5'
with WindX(wtk_file, hsds=True) as f:
    meta = f.meta
    time_index = f.time_index
    wspd_100m = f['windspeed_100m']
```

Note: `WindX` will automatically interpolate to the desired hub-height:

```python
from rex import WindX

wtk_file = '/nrel/wtk/conus/wtk_conus_2010.h5'
with WindX(wtk_file, hsds=True) as f:
    print(f.datasets)  # not 90m is not a valid dataset
    wspd_90m = f['windspeed_90m']
```

`rex` also allows easy extraction of the nearest site to a desired (lat, lon)
location:

```python
from rex import WindX

wtk_file = '/nrel/wtk/conus/wtk_conus_2010.h5'
nwtc = (39.913561, -105.222422)
with WindX(wtk_file, hsds=True) as f:
    nwtc_wspd = f.get_lat_lon_df('windspeed_100m', nwtc)
```

or to extract all sites in a given region:

```python
from rex import WindX

wtk_file = '/nrel/wtk/conus/wtk_conus_2010.h5'
state='Colorado'
with WindX(wtk_file, hsds=True) as f:
    co_wspd = f.get_region_df('windspeed_100m', state, region_col='state')
```

Lastly, `rex` can be used to extract all variables needed to run SAM at a given
location:

```python
from rex import WindX

wtk_file = '/nrel/wtk/conus/wtk_conus_2010.h5'
nwtc = (39.913561, -105.222422)
with WindX(wtk_file, hsds=True) as f:
    nwtc_sam_vars = f.get_SAM_df(nwtc)
```

If you would rather access the WIND Toolkit data directly using h5pyd:

```python
# Extract the average 100m wind speed
import h5pyd
import pandas as pd

# Open .h5 file
with h5pyd.File('/nrel/wtk/conus/wtk_conus_2010.h5', mode='r') as f:
    # Extract meta data and convert from records array to DataFrame
    meta = pd.DataFrame(f['meta'][...])
    # 100m windspeed dataset
    wspd = f['windspeed_100m']
    # Extract scale factor
    scale_factor = wspd.attrs['scale_factor']
    # Extract, average, and unscale windspeed
    mean_wspd_100m = wspd[...].mean(axis=0) / scale_factor

# Add mean windspeed to meta data
meta['Average 100m Wind Speed'] = mean_wspd_100m
```

```python
# Extract time-series data for a single site
import h5pyd
import pandas as pd

# Open .h5 file
with h5pyd.File('/nrel/wtk/conus/wtk_conus_2010.h5', mode='r') as f:
    # Extract time_index and convert to datetime
    # NOTE: time_index is saved as byte-strings and must be decoded
    time_index = pd.to_datetime(f['time_index'][...].astype(str))
    # Initialize DataFrame to store time-series data
    time_series = pd.DataFrame(index=time_index)
    # Extract 100m wind speed, wind direction, temperature, and pressure
    for var in ['windspeed_100m', 'winddirection_100m',
    			'temperature_100m', 'pressure_100m']:
    	# Get dataset
    	ds = f[var]
    	# Extract scale factor
    	scale_factor = ds.attrs['scale_factor']
    	# Extract site 100 and add to DataFrame
    	time_series[var] = ds[:, 100] / scale_factor
```
