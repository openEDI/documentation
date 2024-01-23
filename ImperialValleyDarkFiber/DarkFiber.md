# Imperial Valley Dark Fiber Project Continuous DAS Data

## Description

Whereas permanent seismic networks are sparsely distributed, this dense array seismic data with gauge length 10 m and sampling rate 500 Hz 
was continuously acquired on a ~ 28 km long segment of unused fiber-optic cable buried in the ground for telecommunication using a novel 
technology called “Distributed Acoustic Sensing”. The objective is to demonstrate dark fiber DAS as a tool for basin-scale geothermal 
exploration and monitoring. 

The included DAS data were recorded during two days at the beginning the project. The study area, Imperial Valley in Southern California, 
is a sedimentary basin characterized by intense seismicity and faulting, high heat flow, and deformation over a broad area in a transtensional 
tectonic regime, hosts multiple producing geothermal fields, and is believed to host other hidden geothermal resources. In particular, the 
dark fiber DAS array passes close to the Brawley geothermal field, which was a hidden geothermal resource prior to its discovery. Therefore, 
the DAS dataset provides new, exciting research opportunities in the studies of seismology, tectonics, seismic exploration, and basin-scale 
geothermal exploration and monitoring. 
    
This dataset is continuous array seismic data acquired using distributed acoustic sensing (DAS) and can be used for all applications and methods 
in seismology that can use DAS data. This dataset can be used for analysis of ambient seismic noise and earthquakes in the Imperial Valley. This 
data can provide insights into sources of ambient seismic noise, subsurface seismic velocity structure, short-term spatiotemporal variations in 
the subsurface, source properties, detection and monitoring of earthquakes, and seismic-wave propagation in the Imperial Valley and similar 
sedimentary basins. 

## Directory structure

This dataset contains continuous raw DAS data acquired over a period of two days (November 12-13, 2020) at the beginning of the Imperial 
Valley Dark Fiber Project. It consists 2,880 files of approximately 400 MB each, each labeled with the start time naming scheme DF__UTC_YYYYMMDD_HHMMSS.SSS.h5.

## Data Format

The DAS data files are 1 minute segments of strain rate in .hdf5 format. The technical specifications of the DAS data acquisition are - 6912 channels, 
4 m channel spacing, 10 m gauge length, 500 Hz sample rate. Data in each hdf5 file in the dataset is a 2D array of dimensions 6912 channels x 30000 time 
samples and int16 datatype under the dataset name “Acoustic”. The number of attributes or headers is 83. The timestamp is in the header “GPSTimeStamp”. 

## Code Examples
For a tutorial of accessing and using this data please see the following link:
  - https://github.com/openEDI/documentation/blob/main/ImperialValleyDarkFiber/DarkFiber_Tutorial_Notebook.ipynb

## References

For additional information about the objective of this data, users can reference the following article:
  -  Ajo-Franklin, J, et al. The Imperial Valley Dark Fiber Project: Toward Seismic Studies Using DAS and Telecom Infrastructure for Geothermal Applications. United States. https://doi.org/10.1785/0220220072 

Additional information about the HDF5 file format can be found [here](https://support.hdfgroup.org/HDF5/doc/H5.format.html).

Users of the Dark Fiber DAS data should use the following citation:
  - Ajo-Franklin, Jonathan, Dobson, Patrick, and Rodriguez Tribaldos, Veronica. Imperial Valley Dark Fiber Project Continuous DAS Data. 
    United States: N.p., 10 Nov, 2020. Web. https://gdr.openei.org/submissions/1499.
