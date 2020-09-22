# PoroTomo

These data were collected during March 2016 at Brady Hot Springs, NV. Silixa’s
iDAS (TM) was used for data acquisition with 1.021 m channel spacing and a
guage length of 10 m. Data in this archive are in .sgy and .h5 files with raw
units (radians of optical phase change per time sample) (Miller, et al.).

The files in this dataset use SEG-y-rev1 (see below for documentation). The
data are also available in .h5 or HDF5 file format.

Horizontal DAS (DASH) data collection began 3/8/16, paused, and then started
again on 3/11/2016 and ended 3/26/2016 using zigzag trenched fiber optic
cabels. Vertical DAS (DASV) data collection began 3/17/2016 and ended 3/28/16
using a fiber optic cable through the first 363 m of a vertical well.





## Directory structure

The PoroTomo data is available on AWS S3: s3://nrel-pds-porotomo/. The data is
available in three formats:

### SEG-Y Format:

Files are stored in daily directories labeled using the format YYYYMMDD.
Individual files names include the date and time appended at the end in the
format YYMMDDHHMMSS. Each file respresents 30 s of data.

s3://nrel-pds-porotomo/SEG-Y/DASH:
- size: ~1-2 GB each
- shape: 8721 traces x 30000 samples/trace

s3://nrel-pds-porotomo/SEG-Y/DASV:
- size: ~0.01-0.02 GB each
- shape: 384 traces x 30000 samples/trace

For examples on accessing the SEG-Y files please see our [example notebook](https://github.com/openEDI/documentation/blob/master/PoroTomo/PoroTomo_Distributed_Acoustic_Sensing_(DAS)_Data_SEGY.ipynb)

### HDF5 (.h5) Format:

Files are stored in daily directories labeled using the format YYYYMMDD.
Individual files names include the date and time appended at the end in the
format YYMMDDHHMMSS. Each file respresents 30 s of data, with the exception of
the .h5 files available via HSDS which represent 24 hrs of data.

s3://nrel-pds-porotomo/H5/DASH:
- size: ~1 GB
- shape: 8721 traces x 30000 samples/trace

s3://nrel-pds-porotomo/H5/DASV:
- size: ~1 GB
- shape: 384 traces x 30000 samples/trace

For examples on accessing the HDF5 files please see our [example notebook](https://github.com/openEDI/documentation/blob/master/PoroTomo/PoroTomo_Distributed_Acoustic_Sensing_(DAS)_Data_hdf5.ipynb)

### HSDS Format: HD5F from the cloud

Files are stored in daily .h5 files

DASH:
- Source .h5: s3://nrel-pds-porotomo/H5/DASH
- HSDS: /nrel/porotomo/DASH

DASV:
- Source .h5: s3://nrel-pds-porotomo/H5/DASV
- HSDS: /nrel/porotomo/DASV

For examples on setting up and using HSDS please see our [example notebook](https://github.com/openEDI/documentation/blob/master/PoroTomo/PoroTomo_Distributed_Acoustic_Sensing_(DAS)_Data_hsds.ipynb)

## Data Format

The following datasets are available in HDF5 and HSDS files:

- channel
- crs
- das
- t
- trace
- x
- y
- z

## References:

Users of the PoroTomo data should please cite:
- [Miller, Douglas E., et al. “DAS and DTS at Brady Hot Springs: Observations about Coupling and Coupled Interpretations.” Semantic Scholar, 14 Feb. 2018](pdfs.semanticscholar.org/048f/419e3c2b4de348a7166b13cab3bc0d56afdc.pdf)

Additional information regarding:
- [SEG-Y-rev1 file structure](https://seg.org/Portals/0/SEG/News%20and%20Resources/Technical%20Standards/seg_y_rev1.pdf)
- [.h5 file format](https://support.hdfgroup.org/HDF5/doc/H5.format.html)
- [DAS Data](http://dx.doi.org/10.1093/gji/ggy102 https://www.osti.gov/servlets/purl/1499141)
- [DAS Data](https://pangea.stanford.edu/ERE/pdf/IGAstandard/SGW/2018/Miller.pdf)
- [Metadata](https://gdr.openei.org/submissions/825)
- [DTS Data](https://gdr.openei.org/submissions/853)

