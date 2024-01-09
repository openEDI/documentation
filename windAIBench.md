## Data Description
The floris_data.h5 file is a robust dataset providing 252,500 samples of diverse wind plant layouts operating under a wide range of yawing and atmospheric conditions. This data submission should be considered a benchmark dataset for comparison with new ML approaches. Wind plant layouts were randomly sampled using a specialized Plant Layout Generator (PLayGen). Given a user-specified plant size and average turbine spacing, PLayGen generates a realistic layout that is reflective of one of the four canonical configurations: (i) cluster, (ii) single string, (iii) multiple string, (iv) parallel string. 500 unique wind plant layouts were sampled with the number of turbines randomly sampled from N_turb  ∈[25,200] and the turbine spacing randomly sampled from Δ_turb  ∈[3D,10D], where D denotes the turbine rotor diameter. For each layout 500 different sets of atmospheric conditions with wind speeds and directions were sampled uniformly from u ∈[0,25] m/s and θ∈[0°,360°], respectively, where u is wind speed and θ is wind direction. Turbulence intensity was sampled uniformly between low (6%), medium (8%), and high (10%). For each atmospheric inflow scenario, the individual turbine yaw angles were randomly sampled from a one-sided truncated Gaussian on the interval [0°,30°] oriented relative to wind inflow direction. Given these randomly sampled wind plant layouts, controls, and atmospheric conditions, the power generation outputs were computed using FLORIS, an open-source analytic engineering flow model created by NREL that predicts steady state hub height velocity in normal or yawed operating conditions. The IEA onshore reference turbine, which has a 130 m rotor diameter, a 110 m hub height, and a rated power capacity of 3.4 MW was used as our fixed turbine technology throughout all simulations. 
The data generation process resulted in 250,000 individual samples. We supplement this data by selecting a subset of cases (50 atmospheric conditions from 50 layouts each for a total of 2,500 cases) for which FLORIS was re-run with wake steering control optimization. In these additional 2,500 cases the turbine yaw angles are optimized to maximize total plant power production. The data generation process was completed between X-Y on the Eagle HPC system located at NREL’s Golden, CO Campus. The data was reformatted and preprocessed for OEDI submission in May 2023. The data was generated as part of a broader effort to support the IEA ‘Net Zero by 2050’ roadmap which calls for an 11x increase in wind energy. Meeting these deployment goals raises many engineering challenges. Thus, the Department of Energy is looking to data-driven modeling to help address them. The role of AI/ML in supporting efforts to address these problems requires novel methods to handle modeling complexities. However, AI/ML research in wind energy has been performed in a nonsystematic, ad hoc manner, making it difficult to identify promising approaches and target investments appropriately. Future R&D planning requires a reliable framework for comparing different methods. In AI/ML research, benchmark data sets with well-defined problem statements have enabled consistent comparisons for emerging approaches and systematic ablation studies. Thus, this submission is encompassed in a larger goal to define benchmark data sets, problems, and metrics for wind energy research to facilitate a systematic approach to developing and comparing emerging AI/ML technologies that can inform future research investments.

## HDF5 – Hierarchical Data Format
All data is contained within a singular HDF5 file, floris_data.h5. HDF5 is a Hierarchical Data Format which functions similarly to a file management system. The .h5 file itself is an object that acts as a container, or group, that can hold a variety of heterogeneous data objects or datasets. The two primary object types within a .h5 file are groups and datasets. Groups function similarly to a directory or folder that can contain objects, known as members, such as datasets or other groups. The root group of a .h5 file is denoted by a forward slash /. Objects within a .h5 file are often described with absolute path names, starting from the root group. For example: /Layout000/Scenarios/Scenario000 is the absolute path for the zeroth layout and corresponding zeroth scenario.
For additional information on the HDF5 file format and API please see the documentation below:
https://docs.hdfgroup.org/hdf5/develop/_getting_started.html 

## Data Structure
The floris_data.h5 file is structured as follows:
|-- root (group, 500 members)
|-- LayoutXXX (group, 3 members)
 		|-- Number of Turbines (dataset)
		|-- Scenarios (group, 500 members)
  			|-- ScenarioXXX (group, 6 members)
				|-- Optimal Yaw (group, 3 members)**
					|-- Turbine Power (dataset)
					|-- Turbine Wind Speed (dataset)
					|-- Yaw Angles (dataset)
  				|-- Turbine Power (dataset)
   				|-- Turbine Wind Speed (dataset)
  				|-- Turbulence Intensity (dataset)
   				|-- Wind Direction (dataset)
   				|-- Wind Speed (dataset)
   				|-- Yaw Angles (dataset)
 		|--  Turbines (group, variable number of members)
   			|-- TurbineXXX (group, 4 members)
   				|-- Hub Height (dataset)
   				|-- Rotor Diameter (dataset)
   				|-- X Location (dataset)
   				|-- Location (dataset) 

** The Optimal Yaw Group is only present in 2500 ScenarioXXX groups. These 2500 groups represent the layouts and scenarios where FLORIS was re-run using wake steering and the yaw angles were optimized for power production. The standard datasets within these ScenarioXXX groups represent the FLORIS simulation with no yaw optimization. For convenience and ease of parsing, an opt_yaw_list.csv file has been provided for users to easily identify the LayoutXXX/ScenarioXXX groups which contain the Optimal Yaw Group and corresponding datasets.

In the structure seen above “XXX” is used to indicate there are multiple groups. For the LayoutXXX and ScenarioXXX groups these values range from 000 to 499 indicating 500 groups for each. For the TurbineXXX group these values are variable, indicating a different number of turbines for each LayoutXXX, ranging between 25 to 200 inclusive.

## Data Dictionary
Name	Type	Shape	Data Type	Units
Number of Turbines	Scalar	()	i4, 4-byte integer	N/A
Turbine Power	Vector	(# turbines,)	f4, 4-byte float	W
Turbine Wind Speed	Vector	(# turbines,)	f4, 4-byte float	m/s
Turbulence Intensity	Scalar	()	f4, 4-byte float	N/A
Wind Direction	Scalar	()	f4, 4-byte float	degrees
Wind Speed	Scalar	()	f4, 4-byte float	m/s
Yaw Angles	Vector	(# turbines,)	f4, 4-byte float	degrees
Hub Height	Scalar	()	f4, 4-byte float	m
Rotor Diameter	Scalar	()	f4, 4-byte float	m
X Location	Scalar	()	f4, 4-byte float	m
Y Location	Scalar	()	f4, 4-byte float	m

## Submission Keywords
energy, power, wind, AI, ML, AI/ML, wind plant, benchmark
