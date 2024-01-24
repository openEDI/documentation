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

How the data is stored with in each file including a data dictionary with
- dataset/variable/column names
- units


## Code Examples
- For code examples, users can reference the [Sup3r GitHub Repository](https://github.com/NREL/sup3r/tree/main) which includes examples for configuration and using the data in
  the [/examples/sup3rcc](https://github.com/NREL/sup3r/tree/main/examples/sup3rcc) repository.

## References

Any helpful references other documentation

## Disclaimer and Attribution

Optional additional attributes/disclaimers
