# Super-Resolution for Renewable Energy Resource Data with Climate Change Impacts (Sup3rCC)

## Description

The Super-Resolution for Renewable Energy Resource Data with Climate Change Impacts (Sup3rCC) data is a collection of 4km hourly wind, solar, temperature, 
humidity, and pressure fields for the contiguous United States under climate change scenarios.

Sup3rCC is downscaled Global Climate Model (GCM) data. For example, the initial dataset "sup3rcc_conus_mriesm20_ssp585_r1i1p1f1" is downscaled from MRI 
ESM 2.0 for climate change scenario SSP5 8.5 and variant label r1i1p1f1. The downscaling process was performed using a generative machine learning approach 
called sup3r: Super-Resolution for Renewable Energy Resource Data (linked below as "Sup3r GitHub Repo"). The data includes both historical and future 
weather years, although the historical years represent the historical average climate, not the actual historical weather that we experienced.

The Sup3rCC data is intended to help researchers study the impact of climate change on energy systems with high levels of wind and solar capacity. Please 
note that all climate change data is only a representation of the *possible* future climate and contains significant uncertainty. Analysis of multiple climate 
change scenarios and multiple climate models can help quantify this uncertainty.

## Directory structure

The directory consists of two serially complete collections of hourly 4km wind, solar, temperature, humidity, and pressure fields for the Continental United States
under different climate change scenarios. Sup3rCC is downscaled Global Climate Model (GCM) data. For example, the initial file set tagged "sup3rcc_conus_mriesm20_ssp585_r1i1p1f1"
is downscaled from MRI ESM 2.0 for climate change scenario SSP5 8.5 and variant label r1i1p1f1. The initial file set tagged "sup3rcc_conus_ecearth3_ssp585_r1i1p1f1" is downscaled 
from EC-Earth3 for climate change scenario SSP5 8.5 and variant label r1i1p1f1.

Files within each of these folders are labeled with the file set tag described above, followed by the naming scheme _variable_year.h5. For example: sup3rcc_conus_ecearth3_ssp585_r1i1p1f1_pressure_2015.h5. 
The data includes both historical and future weather years, although the historical years represent the historical average climate, not true historical weather.  

Within the S3 bucket there is also a folder "models" providing pre-trained Sup3rCC generative machine learning models.

## Data Format

The data is provided in high density data file (.h5) separated by year. The variables mentioned in [the Data Format documentation](https://nrel.github.io/rex/misc/examples.nsrdb.html#data-format) are provided in 2 dimensional time-series arrays (called “datasets” in h5 files) with dimensions (time x location). The temporal axis is defined by the time_index dataset, while the positional axis is defined by the meta dataset. We typically refer to a single site in the data with a gid, which is just the index of the site in the meta data (zero-indexed). For storage efficiency each variable has been scaled and stored as an integer. The scale_factor is provided in the scale_factor attribute. The units for the variable data is also provided as an attribute (units).

## Code Examples
- For code examples, users can reference the [Sup3r GitHub Repository](https://github.com/NREL/sup3r/tree/main) which includes examples for configuration and using the data in
  the [/examples/sup3rcc](https://github.com/NREL/sup3r/tree/main/examples/sup3rcc) repository.

## References

Users of the Sup3rCC data should use the following citation:

   - Buster, Grant, Benton, Brandon, Glaws, Andrew, and King, Ryan. Super-Resolution for Renewable Energy Resource Data with Climate Change Impacts (Sup3rCC). United States: N.p., 19 Apr, 2023. Web. doi: 10.25984/1970814.
