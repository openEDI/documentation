# BuildingsBench: A Large-Scale Dataset of 900K Buildings and Benchmark for Short-Term Load Forecasting

## Description

The BuildingsBench datasets consist of:

  - Buildings-900K: A large-scale dataset of 900K buildings for pretraining models on the task of short-term load forecasting (STLF). Buildings-900K is statistically representative of the entire U.S. building stock.
  - 7 real residential and commercial building datasets for benchmarking two downstream tasks evaluating generalization: zero-shot STLF and transfer learning for STLF.

Buildings-900K can be used for pretraining models on day-ahead STLF for residential and commercial buildings. The specific gap it fills is the lack of large-scale and diverse time series datasets of sufficient size 
for studying pretraining and finetuning with scalable machine learning models. Buildings-900K consists of synthetically generated energy consumption time series. It is derived from the NREL End-Use Load Profiles (EULP) 
dataset (see link to this database in the links further below). However, the EULP was not originally developed for the purpose of STLF. Rather, it was developed to "...help electric utilities, grid operators, 
manufacturers, government entities, and research organizations make critical decisions about prioritizing research and development, utility resource and distribution system planning, and state and local energy planning 
and regulation." Similar to the EULP, Buildings-900K is a collection of Parquet files and it follows nearly the same Parquet dataset organization as the EULP. As it only contains a single energy consumption time series 
per building, it is much smaller (~110 GB). 

BuildingsBench also provides an evaluation benchmark that is a collection of various open source residential and commercial real building energy consumption datasets. The evaluation datasets, which are provided alongside 
Buildings-900K, are collections of CSV files which contain annual energy consumption. The size of the evaluation datasets altogether is less than 1GB.

## Directory structure

A README file providing details about how the data is stored and describing the organization of the datasets can be found within each data lake version under BuildingsBench.

- `BuildingsBench/`
    - `Buildings-900K/end-use-load-profiles-for-us-building-stock/2021/`: The Buildings-900K pretraining and validation data.
        - `comstock_amy2018_release_1/`
            - `timeseries_individual_buildings/`
                - `by_puma_midwest`
                    - `upgrade=0`
                        - `puma={puma_id}/*.parquet`
                        - `...`
                - `by_puma_northeast`
                - `by_puma_south`
                - `by_puma_west`
            - `weather/`
                - `amy2018/`
                    - `{puma_id}_2018.csv`
                    - ...
            - `metadata/`
                - `metadata.parquet`
        - `...`: Other datasets            
    - `BDG-2/`: Building Data Genome Project 2 BuildingsBench evaluation data *with outliers removed*. 
        -  `{building_id}={year}.csv`: The .csv files for the BDG-2 dataset.
        - `...`: Other buildings
    - `...`: Other evaluation datasets (Borealis, Electricity, etc.)
    - `buildingsbench_with_outliers`: The BuildingsBench evaluation data *with outliers*.
        - `BDG-2/`: Buildings Data Genome Project 2 BuildingsBench evaluation data *with outliers*. 
            -  `{building_id}={year}.csv`: The .csv files for the BDG-2 dataset.
            - `...`: Other buildings
        - `...`: Other evaluation datasets (Borealis, Electricity, etc.)
    - `LICENSES/`: Licenses for each evaluation dataset redistributed in BuildingsBench. 
    - `metadata/`: Metadata for the evaluation suite.
        - `benchmark.toml`: Metadata for the benchmark. For each dataset, we specify:
            - `building_type`: `residential` or `commercial`.
            - `latlon`: a List of two floats representing the location of the building(s).
            - `conus_location`: The name of the county or city in the U.S. where the building is located, or a county/city in the U.S. of similar climate to the building's true location.
            - `actual_location`: County/city where the building actually is located. This will be different from `conus_location` when the building is outside of the CONUS. These values are for book-keeping and can be set to dummy values.
            - `url`: The URL where the dataset was obtained from.
        - `building_years.txt`: List of .csv files included in the benchmark. Each line is of the form `{dataset}/{building_id}={year}.csv`.
        - `withheld_pumas.tsv`: List of PUMAs withheld from the training/validation set of Buildings-900K, which we use as synthetic test data.
        - `map_of_pumas_in_census_region*.csv`: Maps PUMA IDs to their geographical centroid (lat/lon).
        - `spatial_tract_lookup_table.csv`: Mapping between census tract identifiers and other geographies.
        - `list_oov.py`: Python script to generate a list of buildings that are OOV for the Buildings-900K tokenizer.
        - `oov.txt`: List of buildings that are OOV for the Buildings-900K tokenizer.
        - `transfer_learning_commercial_buildings.txt`: List of 100 commercial buildings from the benchmark we use for evaluating transfer learning.
        - `transfer_learning_residential_buildings.txt`: List of 100 residential buildings from the benchmark we use for evaluating transfer learning.
        - `transfer_learning_hyperparameter_tuning.txt`: List of 2 held out buildings (1 commercial, 1 residential) that can be used for hyperparameter tuning.
        - `train*.idx`: Index files for fast dataloading of Buildings-900K. This file uncompressed is 16GB. 
        - `val*.idx`: Index files for fast dataloading of Buildings-900K.
        - `transforms`: Directory for storing data transform info.

## Data Format

### Parquet file format

  The pretraining dataset Buildings-900K is stored as a collection of PUMA-level parquet files.
  Each parquet file in Buildings-900K is stored in a directory named after a unique PUMA ID `puma={puma_id}/*.parquet`. The first column is the timestamp and 
  each subsequent column is the energy consumption in kWh for a different building in that. These columns are named by building id. The timestamp is in the 
  format `YYYY-MM-DD HH:MM:SS`. The energy consumption is in kWh.
  The parquet files are compressed with snappy.

### CSV file format

Most CSV files in the benchmark are named `building_id=year.csv` and correspond to a single building's energy consumption time series. The first column is the 
timestamp (the Pandas index), and the second column is the energy consumption in kWh. The timestamp is in the format `YYYY-MM-DD HH:MM:SS`. The energy consumption is in kWh. 

Certain datasets have multiple buildings in a single file. In this case, the first column is the timestamp (the Pandas index), and each subsequent column is the energy 
consumption in kWh for a different building. These columns are named by building id. The timestamp is in the format `YYYY-MM-DD HH:MM:SS`. The energy consumption is in kWh.

## Code Examples
For dataset quick start and other tutorials see the [BuildingsBench Github Tutorials](https://github.com/NREL/BuildingsBench/tree/main/tutorials).

## References

- [NeurIPS paper](https://arxiv.org/abs/2307.00142) providing additional information on the datasets along with the analyses conducted with BuildingsBench.
- [End-Use Load Profiles (EULP)](https://data.openei.org/submissions/4520) from which Buildings-900K is derived from.
- Additional information about the parquet file format can be found [here](https://parquet.apache.org/).

Users of the BuildingsBench data should please cite:
- Emami, Patrick, Graf, Peter. BuildingsBench: A Large-Scale Dataset of 900K Buildings and Benchmark for Short-Term Load Forecasting. United States: N.p., 31 Dec, 2018. Web. doi: 10.25984/1986147.

