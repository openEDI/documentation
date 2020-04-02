# PoroTomo
PoroTomo DAS data in both SEG-Y and .h5 formats with tutorial notebook for use.

These data were collected during March 2016 at Brady Hot Springs, NV. Silixa’s iDAS (TM) was used for data acquisition 
with 1.021 m channel spacing and a guage length of 10 m. Data in this archive are in .sgy and .h5 files with raw units 
(radians of optical phase change per time sample) (Miller, et al.).

The files in this dataset use SEG-y-rev1 (see below for documentation). The data are also available in .h5 or HDF5 file format. 

Horizontal DAS (DASH) data collection began 3/8/16, paused, and then started again on 3/11/2016 and ended 3/26/2016 using zigzag trenched fiber optic cabels. Vertical DAS (DASV) data collection began 3/17/2016 and ended 3/28/16 using a fiber optic cable through the first 363 m of a vertical well. 

Files are stored in daily directories labeled using the format YYYYMMDD. Individual files names include the date and time appended at the end in the format YYMMDDHHMMSS. Each file respresents 30 s of data. 

DASH SEG-Y files:
    size: ~1-2 GB each
    shape: 8721 traces x 30000 samples/trace
DASV SEG-Y files: 
    size: ~0.01-0.02 GB each 
    shape: 384 traces x 30000 samples/trace.
DASH .h5 files:
    size: ~1 GB
DASV .h5 files:
    size: ~1 GB

References:

Additional details about the DAS survey: 
    Miller, Douglas E., et al. “DAS and DTS at Brady Hot Springs: Observations about Coupling and Coupled 
    Interpretations.” Semantic Scholar, 14 Feb. 2018, pdfs.semanticscholar.org/048f/419e3c2b4de348a7166b13cab3bc0d56afdc.pdf.
    
Additional info about the SEG-Y-rev1 file structure: https://seg.org/Portals/0/SEG/News%20and%20Resources/Technical%20Standards/seg_y_rev1.pdf

Additional info about .h5 file format: https://support.hdfgroup.org/HDF5/doc/H5.format.html

Additional URLs pointing to documentation of the structure and content of the dataset: 
Structure: https://github.com/openEDI/documentation/blob/master/PoroTomo.md
Content: http://dx.doi.org/10.1093/gji/ggy102 https://www.osti.gov/servlets/purl/1499141 
https://pangea.stanford.edu/ERE/pdf/IGAstandard/SGW/2018/Miller.pdf
Metadata: https://gdr.openei.org/submissions/825
DTS Data: https://gdr.openei.org/submissions/853

