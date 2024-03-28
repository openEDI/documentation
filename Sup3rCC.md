# Super-Resolution for Renewable Energy Resource Data with Climate Change Impacts (Sup3rCC)

## Description

The Super-Resolution for Renewable Energy Resource Data with Climate Change Impacts (Sup3rCC) data is a collection of 4km hourly wind, solar, temperature, humidity, and pressure fields for the contiguous United States under various climate change scenarios.

Sup3rCC is downscaled Global Climate Model (GCM) data. The downscaling process was performed using a generative machine learning approach called sup3r: Super-Resolution for Renewable Energy Resource Data (linked below as "Sup3r GitHub Repo"). The data includes both historical and future weather years, although the historical years represent the historical climate, not the actual historical weather that we experienced. You cannot use Sup3rCC data to study historical weather events, although other sup3r datasets may be intended for this.

The Sup3rCC data is intended to help researchers study the impact of climate change on energy systems with high levels of wind and solar capacity. Please note that all climate change data is only a representation of the *possible* future climate and contains significant uncertainty. Analysis of multiple climate change scenarios and multiple climate models can help quantify this uncertainty.

## Version Log
The Sup3rCC data has versions that coincide with the sup3r software versions. Note that not every sup3r software version will have a corresponding Sup3rCC data release, but every Sup3rCC data release will have a corresponding sup3r software version. This table records versions of Sup3rCC data releases. Sup3rCC generative models may have slightly different versions than the data. The version in the Sup3rCC .h5 file attribute can be inspected to verify the actual version of the data you are using.

| Version | Date | Notes |
| -------- | -------- | ------- |
| 0.1.0 | 6/27/2023	| Initial Sup3rCC release with two GCMs and one climate scenario. Known issues: few years used for bias correction, simplistic GCM bias correction method, mean bias in high-res output especially in wind and solar data, imperfect wind diurnal cycles when compared to WTK and timing of diurnal peak temperature when compared to observation. 

## Directory structure

The Sup3rCC directory contains downscaled data for multiple projections of future climate change. For example, a file from the initial data release `sup3rcc_conus_ecearth3_ssp585_r1i1p1f1_wind_2015.h5` is downscaled from the climate model MRI ESM 2.0 for climate change scenario SSP5 8.5 and variant label r1i1p1f1. The file contains wind variables for the year 2015. Note that this will represent the climate from 2015, but not the actual weather we experienced. 

Within the S3 bucket there is also a folder `models` providing pre-trained Sup3rCC generative machine learning models. See the Sup3r GitHub Repo below for examples of how to use these models. 

## Data Format

The data is provided in Hierarchical Data Format (.h5) separated by year and by variable set. Variables are provided in 2 dimensional spatiotemporal arrays (called “datasets” in h5 files) with dimensions `(time, space)`. The temporal axis is defined by the `time_index` dataset, while the positional axis is defined by the `meta` dataset. Additional details on data format and data access patterns can be found in the [rex docs](https://nrel.github.io/rex/misc/examples.nrel_data.html).

## Code Examples
- For code examples, users can reference the [Sup3r GitHub Repository](https://github.com/NREL/sup3r/tree/main) which includes examples for Sup3rCC data access in the [/examples/sup3rcc](https://github.com/NREL/sup3r/tree/main/examples/sup3rcc) directory.
- The [rex docs](https://nrel.github.io/rex/misc/examples.nrel_data.html) provide examples on the easiest ways to access the data remotely or on the NREL HPC. 

## References
Users of the Sup3rCC data should use the following citation:

   - Buster, Grant, Benton, Brandon, Glaws, Andrew, and King, Ryan. “High-Resolution Meteorology with Climate Change Impacts from Global Climate Model Data Using Generative Machine Learning.” _Nature Energy_, March 14, 2024. https://doi.org/10.1038/s41560-024-01507-9.
   - Buster, Grant, Benton, Brandon, Glaws, Andrew, and King, Ryan. Super-Resolution for Renewable Energy Resource Data with Climate Change Impacts (Sup3rCC). United States: N.p., 19 Apr, 2023. Web. doi: 10.25984/1970814.
