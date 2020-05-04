# High Resolution Ocean Surface Wave Hindcast

## Description

The development of this dataset was funded by the U.S. Department of Energy,
Office of Energy Efficiency & Renewable Energy, Water Power Technologies Office
to improve our understanding of the U.S. wave energy resource and to provide
critical information for wave energy project development and wave energy
converter (WEC) conceptual design. This will be the highest resolution publicly
available long-term wave hindcast dataset that – when complete – covers the
entire U.S. EEZ. As such, it could be of value to any company with marine
operations inside the U.S. EEZ. Specifically, the data can be used to
investigate the historical record of wave statistics at any U.S. site. This
level of detail could be of interest to the Oil and Gas industry for offshore
platform engineering, to the offshore wind industry for turbine and array
design, to offshore aquaculture production and blue economy development, to
coastal communities for extreme hazards mitigation,  to global shipping
companies and fisherman for a better understanding of weather windows and
seasonal wave climate patterns at a spatial resolution that does not exist
elsewhere. The NREL Offshore Wind group has expressed significant interest in
this dataset for device structural modeling, array design, and economic
modeling.

## Model

This is the highest resolution publicly available wave hindcast dataset. The
multi-scale, unstructured-grid modeling approach using WaveWatch III and SWAN
enabled long-term (decades) high-resolution hindcasts in a large regional
domain. The model was extensively validated not only for the most common wave
parameters, but also six IEC resource parameters and 2D spectra with high
quality spectral data derived from publicly available buoys. This creation of
this dataset was funded by the U.S. Department of Energy, Office of Energy
Efficiency & Renewable Energy, Water Power Technologies Office under Contract
DE-AC05-76RL01830 to Pacific Northwest National Laboratory (PNNL). Additional
details on detailed definitions of the variables found in the dataset, the
SWAN and WaveWatch III model configuration and model validation are available
in a peer-review publication
[Development and validation of a high-resolution regional wave hindcast model for U.S. West Coast wave resource characterization](https://www.osti.gov/biblio/1599105)
and a PNNL technical report:
[High-Resolution Regional Wave Hindcast for the U.S.West Coast](https://www.osti.gov/biblio/1573061/).
This study was funded by the U.S. Department of Energy, Office of Energy
Efficiency & Renewable Energy, Water Power Technologies Office under Contract
DE-AC05-76RL01830 to Pacific Northwest National Laboratory (PNNL).

The following variables were extracted from the SWAM Model data and are
available in the .h5 files:
- Mean Wave Direction: Direction Normal to the Wave Crests
- Significant Wave Height: Calculated as the zeroth spectral moment (i.e., H_m0)
- Mean Absolute Period: Resolved Spectral Moment (m_0/m_1)
- Peak Period: The period associated with the maximum value of the wave energy spectrum
- Mean Zero Crossing Period: Total wave energy flux from all directions
- Energy Period: Spectral width characterizes the relative spreading of energy in the wave spectrum.
- Directionality Coefficient: Fraction of total wave energy travelling in the direction of maximum wave power
- Maximum Energy Direction: The direction from which the most wave energy is travelling
- Omni-Directional Wave Power: Total wave energy flux from all directions
- Spectral Width: Spectral width characterizes the relative spreading of energy in the wave spectrum.

The dataset currently covers the U.S. Exclusive Economic Zone (‘EEZ’, up to
200 nautical miles from shore) offshore of the West Coast, and includes shallow
nearshore regions not covered by previous model hindcasts. Future additions to
the dataset will extend the coverage to the entire U.S. EEZ, including Island
territories. The dataset has a 3-hour timestep spanning 32 years from 1979
through 2010. It includes the most common wave statistics (wave height, wave
direction, wave period), alongside several other wave statistics developed for
the wave energy sector. The dataset was generated from the unstructured-grid
SWAN model output that was driven by a WaveWatch III model with global-regional
nested grids. The SWAN model simulations were performed with a spatial
resolution as fine as 200 meters in shallow waters:

- West Coast United States: Dataset Available
- East Coast United States: Available soon
- Alaskan Coast: Available soon

## Directory structure

High Resolution Ocean Surface Wave Hindcast data is made available as a series
of hourly .h5 located on AWS S3:
- `s3://WPTO-pds-US-wave/v1.0.0`


The US wave data is also available via HSDS at /nrel/US_Wave
For examples on setting up and using HSDS please see our [examples repository](https://github.com/nrel/hsds-examples)

## Data Format

The data is provided in high density data file (.h5) separated by year. The
variables mentioned above are provided in 2 dimensional time-series arrays with
dimensions (time x location). The temporal axis is defined by the `time_index`
dataset, while the positional axis is defined by the `coordinate` dataset. The
units for the variable data is also provided as an attribute (`units`). The
SWAN and IEC valiable names are also provide under the attributes
(`SWAWN_name`) and (`IEC_name`) respectively.

## Python Examples

Example scripts to extract wind resource data using python are provided below:

The easiest way to access and extract data from the Resource eXtraction tool
[`rex`](https://github.com/nrel/rex)


```python
from rex import ResourceX

wave_file = '/nrel/US_Wave/US_wave_2010.h5'
with ResourceX(wave_file, hsds=True) as f:
    meta = f.meta
    time_index = f.time_index
    swh = f['significant_wave_height']
```

`rex` also allows easy extraction of the nearest site to a desired (lat, lon)
location:

```python
from rex import ResourceX

wave_file = '/nrel/US_Wave/US_wave_2010.h5'
lat_lon = (34.399408, -119.841181)
with ResourceX(wave_file, hsds=True) as f:
    lat_lon_swh = f.get_lat_lon_df('significant_wave_height', nwtc)
```

or to extract all sites in a given region:

```python
from rex import ResourceX

wave_file = '/nrel/US_Wave/US_wave_2010.h5'
jurisdication='California'
with ResourceX(wave_file, hsds=True) as f:
    ca_swh = f.get_region_df('significant_wave_height', jurisdiction,
                             region_col='jurisdiction')
```

If you would rather access the US Wave data directly using h5pyd:

```python
# Extract the average wave height
import h5pyd
import pandas as pd

# Open .h5 file
with h5pyd.File('/nrel/US_Wave/US_wave_2010.h5', mode='r') as f:
    # Extract meta data and convert from records array to DataFrame
    meta = pd.DataFrame(f['meta'][...])
    # Significant Wave Height
    swh = f['significant_wave_height']
    # Extract scale factor
    scale_factor = swh.attrs['scale_factor']
    # Extract, average, and unscale wave height
    mean_swh = swh[...].mean(axis=0) / scale_factor

# Add mean windspeed to meta data
meta['Average Wave Height'] = mean_swh
```

```python
# Extract time-series data for a single site
import h5pyd
import pandas as pd

# Open .h5 file
with h5pyd.File('/nrel/US_Wave/US_wave_2010.h5', mode='r') as f:
    # Extract time_index and convert to datetime
    # NOTE: time_index is saved as byte-strings and must be decoded
    time_index = pd.to_datetime(f['time_index'][...].astype(str))
    # Initialize DataFrame to store time-series data
    time_series = pd.DataFrame(index=time_index)
    # Extract wave height, direction, and period
    for var in ['significant_wave_height', 'mean_wave_direction',
                'mean_absolute_period']:
    	# Get dataset
    	ds = f[var]
    	# Extract scale factor
    	scale_factor = ds.attrs['scale_factor']
    	# Extract site 100 and add to DataFrame
    	time_series[var] = ds[:, 100] / scale_factor
```
## References

Users of the High Resolution Ocean Surfae Wave Hindcast should use the
following citations:
- [Wu, Wei-Cheng, et al. "Development and validation of a high-resolution regional wave hindcast model for US West Coast wave resource characterization." Renewable Energy 152 (2020): 736-753.](https://www.osti.gov/biblio/1599105)
- [Yang, Zhaoqing, et al. High-Resolution Regional Wave Hindcast for the US West Coast. No. PNNL-28107. Pacific Northwest National Lab.(PNNL), Richland, WA (United States), 2018.](https://www.osti.gov/biblio/1573061/)