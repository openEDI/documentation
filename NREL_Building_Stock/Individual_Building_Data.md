# Downloading individual building data files from ComStock results

For many use cases, the goal is to aggregate the timeseries energy data
from many buildings in a given region, building type, etc. The methods For
doing that are documented in [TODO link to query document].  Those methods
rely on AWS Athena because of the sheer size of the data to be aggregated.

Other use cases may require access the individual building timeseries data.
This document describes how that data is stored and how to access it.

## Requirements

An AWS account is necessary to follow this tutorial.

## Data location

The ComStock dataset is stored in a publicly-accessible Amazon S3 bucket.
To access this bucket:

1. Login to AWS
2. Visit the [ComStock data bucket](https://s3.console.aws.amazon.com/s3/buckets/nrel-pds-building-stock?region=us-west-2&tab=objects)

This should take you to an interface where you can browse through the directories
and look through the data.

## Data organization

```
nrel-pds-building-stock
├── athena # root directory
├──── 2020 # year the dataset was published
├────── comstock_v1 # name of the dataset
├──────── metadata # building characteristics and annual energy data
│         ├── fast1_metadata.parquet # read all files to get all buildings
│         ├── fast2_metadata.parquet
│         └── ...
├────────climate_zone # timeseries data, partitioned by climate zone
│         ├── upgrade=0
│         ├───── climate_zone=1A
│         ├──────── 100022-0.parquet # buildingID-upgradeID
│         ├──────── 100052-0.parquet
│         ├──────── 10006-0.parquet
│         └──────── ...
├────────state # timeseries data
│         ├── upgrade=0
│         ├───── state=01 # same timeseries data, partitioned by state
│         ├──────── 100022-0.parquet
│         ├──────── 100052-0.parquet
│         ├──────── 10006-0.parquet
└         └──────── ...
```
## Finding specific buildings

In order to find specific buildings, first the metadata files should be
downloaded and parsed.  The list of buildings can be filtered by various
characteristics, and a list of building IDs can be generated.  Either the
state or climate_zone should be determined for each building ID.

Once the list of building IDs and corresponding state or is ready, the
individual files can be retrieved from either the `/state` or `/climate_zone`
directories.  The timeseries profiles in these directories are identical, they
are duplicated for query optimization. The full URI to a file will look like:
```
s3://nrel-pds-building-stock/comstock/athena/2020/comstock_v1/state/upgrade=0/state=01/100094-0.parquet
```
## Programatic download of files
The files can be downloaded programatically using a Python library such as
[Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html),
or by using a similar library in another programming language.

OEDI has created a set of tools to facilitate access to open energy data sets, including ComStock. Please visit the [open-data-access-tools documentation page](https://openedi.github.io/open-data-access-tools/) for more info. You can find jupyter notebook examples that show how to use the tools in our [examples repository](https://github.com/openEDI/open-data-access-tools/tree/integration/examples).
