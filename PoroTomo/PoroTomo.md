# PoroTomo

The data were collected during March 2016 at the PoroTomo Natural Laboratory in Brady's Hot 
Springs, Nevada. 

Silixa’s iDAS (TM) was used for DAS data acquisition with 1.021 m channel spacing and a
guage length of 10 m. Data in this archive are in .sgy and .h5 files with raw
units (radians of optical phase change per time sample) (Miller, et al.).

The files in this dataset use SEG-y-rev1 (see below for documentation). The
data are also available in .h5 or HDF5 file format.

Horizontal DAS (DASH) data collection began 3/8/2016, paused, and then started
again on 3/11/2016 and ended 3/26/2016 using zigzag trenched fiber optic
cabels. Vertical DAS (DASV) data collection began 3/17/2016 and ended 3/28/2016
using a fiber optic cable through the first 363 m of a vertical well.

The nodal seismometer data consists of continuous and windowed (to vibroseis sweep) SAC files. Nodal data collection began between 3/6/2016 and 3/11/2016 depending on the station, and ended between 3/26/2016 and 3/28/2016 also depending on the station. Station names, locations, start times, stop times, and orientations can be found in the nodal seismometer metadata linked below.

## DAS

### Directory structure

The PoroTomo DAS data is available on AWS S3: s3://nrel-pds-porotomo/DAS/. The data is
available in three formats:

#### SEG-Y Format:

Files are stored in daily directories labeled using the format YYYYMMDD.
Individual files names include the date and time appended at the end in the
format YYMMDDHHMMSS. Each file respresents 30 s of data.

s3://nrel-pds-porotomo/DAS/SEG-Y/DASH:
- size: ~1-2 GB each
- shape: 8721 traces x 30000 samples/trace

s3://nrel-pds-porotomo/DAS/SEG-Y/DASV:
- size: ~0.01-0.02 GB each
- shape: 384 traces x 30000 samples/trace

For examples on accessing the SEG-Y files please see our [example notebook](https://github.com/openEDI/documentation/blob/master/PoroTomo/PoroTomo_Distributed_Acoustic_Sensing_(DAS)_Data_SEGY.ipynb)

#### HDF5 (.h5) Format:

Files are stored in daily directories labeled using the format YYYYMMDD.
Individual files names include the date and time appended at the end in the
format YYMMDDHHMMSS. Each file respresents 30 s of data, with the exception of
the .h5 files available via HSDS which represent 24 hrs of data.

s3://nrel-pds-porotomo/DAS/H5/DASH:
- size: ~1 GB
- shape of 'das' variable: 8721 traces x 30000 samples/trace

s3://nrel-pds-porotomo/DAS/H5/DASV:
- size: ~0.04 GB
- shape of 'das' variable: 384 traces x 30000 samples/trace

For examples on accessing the HDF5 files please see our [example notebook](https://github.com/openEDI/documentation/blob/master/PoroTomo/PoroTomo_Distributed_Acoustic_Sensing_(DAS)_Data_hdf5.ipynb)

#### HSDS Format: HD5F from the cloud

Files are stored in daily .h5 files

DASH:
- Source .h5: s3://nrel-pds-porotomo/H5/DASH
- HSDS: /nrel/porotomo/DASH

DASV:
- Source .h5: s3://nrel-pds-porotomo/H5/DASV
- HSDS: /nrel/porotomo/DASV

For examples on setting up and using HSDS please see our [example notebook](https://github.com/openEDI/documentation/blob/master/PoroTomo/PoroTomo_Distributed_Acoustic_Sensing_(DAS)_Data_hsds.ipynb)

### Data Format

The following datasets are available in HDF5 and HSDS files:

- channel: channel number (along cabel)
- crs: coordinate reference system
- das: 2D array with das data (shape: t x channel)
- t: time in µs with respect to start of survey
- trace: enumerated integers over length of the trace
- x: x position of channel
- y: y position of channel
- z: z position of channel

## Nodal Seismometer Data

### Directory Structure

The PoroTomo Nodal Seismometer data is available on AWS S3: s3://nrel-pds-porotomo/Nodal/. 
The following data and metadata are available:

#### Continuous Data

SAC files of the continuous raw data from the nodal seismometers. Data files sorted into 
folders by seismometer station number. Note: no data recovered from stations 73 and 82.

s3://nrel-pds-porotomo/Nodal/nodal_sac:
- size: 45-173 MB


#### Field Notes and Metadata

PDF scans of field notes and metadata for nodal seismometers including instrument 
installation and recovery.

s3://nrel-pds-porotomo/Nodal/nodal_metadata:
- size: 1.3-15.5 MB

#### P-Picks

P-wave travel times auto-picked from cross-correlation waveforms. The files list the 
source time and location for a vibe sweep stack followed by the travel time to each nodal 
instrument and two means of pick quality assessment. See README.txt for details.

s3://nrel-pds-porotomo/Nodal/nodal_analysis/p_picks:
- size: 1.3-1.4 MB

#### Sweep Data

29.8 second long SAC files of the raw nodal seismometer data starting 3.9 seconds before the 
initiation of each vibroseis sweep extracted from continuous 24 hour files. Data files sorted 
into folders by sweep number (see GDR Submission 826).

s3://nrel-pds-porotomo/Nodal/nodal_sac_sweep:
- size: 58.8 kB

## References:

Users of the PoroTomo data should please cite:
- [Miller, Douglas E., et al. “DAS and DTS at Brady Hot Springs: Observations about Coupling and Coupled Interpretations.” Semantic Scholar, 14 Feb. 2018](pdfs.semanticscholar.org/048f/419e3c2b4de348a7166b13cab3bc0d56afdc.pdf)

Additional information regarding:
- [SEG-Y-rev1 file structure](https://seg.org/Portals/0/SEG/News%20and%20Resources/Technical%20Standards/seg_y_rev1.pdf)
- [.h5 file format](https://support.hdfgroup.org/HDF5/doc/H5.format.html)
- [DAS Data](http://dx.doi.org/10.1093/gji/ggy102)
- [PoroTomo Technical Report](https://www.osti.gov/servlets/purl/1499141)
- [DAS and DTS Interpretation](https://pangea.stanford.edu/ERE/pdf/IGAstandard/SGW/2018/Miller.pdf)
- [DTS and DAS Metadata](https://gdr.openei.org/submissions/825)
- [DTS Data](https://gdr.openei.org/submissions/853)
- [Nodal Seismometer Metadata](https://gdr.openei.org/submissions/826)
