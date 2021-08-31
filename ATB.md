# Annual Technology Baseline (ATB)

## Description

The NREL Annual Technology Baseline (ATB) provides a consistent set of technology and performance data for energy analysis. This dataset was developed with funding from the U.S. Department of Energy's,
Office of Energy Efficiency & Renewable Energy.

To inform electric and transportation sector analysis in the United States, each year NREL provides a robust set of modeling input assumptions for energy technologies, and a diverse set of potential electricity generation futures or modeling scenarios (Standard Scenarios).

The ATB is a populated framework to identify technology-specific cost and performance paramters or other investment decision metrics across a range of fuel price conditions as well as site-specific conditions for electric generation technologies at present and with projections through 2050.  

## Model

The purpose of the ATB is to provide CAPEX, O&M, and capacity factor estimates for Base Year and future year projections representing three levels of technical innovation (conservative, moderate, and advanced) for use in electric sector models.

The R&D Only cases are intended to reflect fundamental technology changes over time — not short-term market variations in pricing, not changes in interest rates or other project finance elements, and not macroeconomic influences such as commodity price fluctuations. These cases attempt to estimate the potential effects of technology innovation across the renewable electricity generation technologies under comparable levels of probability. This is inherently uncertain.

The Market + Policies Case approximate the costs of electricity generation plants with Independent Power Producer financial terms, covering the energy component of electric system planning and operation. Important items that are not included in these costs limit the validity of comparisons across technologies. The following table summarizes these limitations, identifies other analyses, tools, and data sets that are more complete sources for these items, and suggests applications that are affected by these limitations of the ATB.

See [technical limitations on the ATB website](https://atb.nrel.gov/electricity/2021/technical_limitations) for more detailed information.

## Directory structure

The CSV files summarize in database-friendly form the capital expenditures, operations expenditures, and capacity factor, as well as the financial assumptions and the levelized cost of energy, for each technology. They are reformatted from the summary section of the spreadsheet, which documents the underlying calculations and data. The same data is also available in the Apache Parquet format.

The files are stored by type and then by year. The file types are parquet and csv. The files can be accessed in the [Department of Energy's Open Energy Data Initiative (OEDI) in the Registry of Open Data on AWS](https://registry.opendata.aws/oedi-data-lake/), or in the [bucket viewer](https://data.openei.org/s3_viewer?bucket=oedi-data-lake&prefix=ATB%2F).

- `s3://oedi-data-lake/ATB/electricity`

## Vintage

Annual data for 2015 through the previous year can be found at [https://atb.nrel.gov/archive](https://atb.nrel.gov/archive) 

The current year data can be found at [https://atb.nrel.gov/data](https://atb.nrel.gov/data) 

## Data Format

The most recent annual data is provided in CSV and Apache Parquet format. The data structure is as follows:

Column | Type | Description
-- | -- | --
  `revision` | bigint | database revision      
  `atb_year` | bigint | year of ATB publication 
  `core_metric_key` | string | concatenated unique key  
  `core_metric_parameter` | string | technology and cost performance parameters  
  `core_metric_case` | string | financial case (R&D or Market)
  `crpyears` | bigint | cost recovery period, years
  `technology` | string | technology
  `techdetail` | string | technology specific classifications and sub-groups   
  `scenario` | string | moderate, conservative and advanced
  `core_metric_variable`| string | projected year 
  `units` | string | units
  `value` | double | value 

## Python Examples

```python

import pandas as pd
from pyathena import connect

conn = connect(
    s3_staging_dir='s3://<USER DEFINED STAGING>/', ##user defined staging directory
    region_name='us-west-2',
    work_group='<USER SPECIFIC WORKGROUP>'  ##specify workgroup if exists
)

df = pd.read_sql("SELECT distinct technology, techdetail FROM oedi-data-lake. limit 8;",conn)
```

For jupyter notebook example see our notebook which includes partitions and data dictionary: [examples repository](https://github.com/openEDI/open-data-access-tools/tree/integration/examples)

## References

Please cite the most relevant publication below when referencing this dataset:

1) NREL (National Renewable Energy Laboratory). 2021. "2021 Annual Technology Baseline." Golden, CO: National Renewable Energy Laboratory. NREL (National Renewable Energy Laboratory). 2021. "2021 Annual Technology Baseline." Golden, CO: National Renewable Energy Laboratory. [https://atb.nrel.gov/](https://atb.nrel.gov/).

## Errata

Documentaion for the ATB errata can be found on the ATB website:

[https://atb.nrel.gov/electricity/2021/errata](https://atb.nrel.gov/electricity/2021/errata)

The Data in the S3 bucket maintains the most up to date version of the parquet and csv files for the years provided.    

## Disclaimer and Attribution

DISCLAIMER AGREEMENT

These detailed electricity generation technology cost and performance data ("Data") are provided by the National Renewable Energy Laboratory ("NREL"), which is operated by the Alliance for Sustainable Energy LLC ("Alliance") for the U.S. Department of Energy (the "DOE").

It is recognized that disclosure of these Data are provided under the following conditions and warnings: (1) these Data have been prepared for reference purposes only; (2) these Data consist of forecasts, estimates, or assumptions made on a best-efforts basis, based upon expectations of current and future conditions at the time they were developed; and (3) these Data were prepared with existing information and are subject to change without notice.

The user understands that DOE/NREL/ALLIANCE are not obligated to provide the user with any support, consulting, training or assistance of any kind with regard to the use of the Data or to provide the user with any updates, revisions or new versions thereof. DOE, NREL, and ALLIANCE do not guarantee or endorse any results generated by use of the Data, and user is entirely responsible for the results and any reliance on the results or the Data in general.

The user understands that DOE/NREL/ALLIANCE are not obligated to provide the user with any support, consulting, training or assistance of any kind with regard to the use of the Data or to provide the user with any updates, revisions or new versions thereof. DOE, NREL, and ALLIANCE do not guarantee or endorse any results generated by use of the Data, and user is entirely responsible for the results and any reliance on the results or the Data in general.

USER AGREES TO INDEMNIFY DOE/NREL/ALLIANCE AND ITS SUBSIDIARIES, AFFILIATES, OFFICERS, AGENTS, AND EMPLOYEES AGAINST ANY CLAIM OR DEMAND, INCLUDING REASONABLE ATTORNEYS' FEES, RELATED TO USER'S USE OF THE DATA. THE DATA ARE PROVIDED BY DOE/NREL/ALLIANCE "AS IS," AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL DOE/NREL/ALLIANCE BE LIABLE FOR ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER, INCLUDING BUT NOT LIMITED TO CLAIMS ASSOCIATED WITH THE LOSS OF DATA OR PROFITS, THAT MAY RESULT FROM AN ACTION IN CONTRACT, NEGLIGENCE OR OTHER TORTIOUS CLAIM THAT ARISES OUT OF OR IN CONNECTION WITH THE ACCESS, USE OR PERFORMANCE OF THE DATA.
>>>>>>> Stashed changes
