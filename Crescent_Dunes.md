
Date	UTC Time	Range gate	Measurement range	Averages	Scan type 1	Elevation range 1	Elevation resolution 1	Azimuth range 1	Azimuth resolution 1	Scan type 2	Elevation range 2	Elevation resolution 2	Azimuth range 2	Azimuth resolution 2	Notes
2024-09-04	19:30	18 m	7200 m	3000	PPI	[0,0]	N/A	[0,90]	1 degree	RHI	[0,5]	0.1 degree	[45,45]	N/A	Original scan pattern
2024-09-05	17:00	18 m	7200 m	3000	PPI	[0,0]	N/A	[0,90]	1 degree	RHI	[0,5]	0.1 degree	[45,45]	N/A	Changed time to UTC, removed gate overlapping
2024-09-12	16:30	18 m	7200 m	3000	PPI	[180,180]	N/A	[180,270]	1 degree	RHI	[0,5]	0.1 degree	[45,45]	N/A	Flip scanning head during PPI to minimize obstacle interference
2024-09-12	18:00	18 m	7200 m	3000	PPI	[180,180]	N/A	[180,270]	1 degree	RHI	[0,5]	0.1 degree	[64,64]	N/A	Align RHI with predominant wind direction and met towers
2024-09-12	20:00	18 m	7200 m	3000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[0,5]	0.1 degree	[64,64]	N/A	Exclude obstacle from PPI scan
2024-09-19	22:30	12 m	3480 m	3000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[0,5]	0.1 degree	[64,64]	N/A	Increase resolution and remove unusable part of range
2024-09-20	21:30	12 m	3480 m	3000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[0,5]	0.05 degree	[64,64]	N/A	Increase angular resolution of RHI
2024-09-20	22:30	12 m	3480 m	3000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[-2,5]	0.05 degree	[64,64]	N/A	Include negative angles in RHI to intersect ground
2024-09-20	23:45	18 m	3600 m	3000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[-2,5]	0.05 degree	[64,64]	N/A	Reduce resolution to improve data quality
2024-12-05	19:00	18 m	3600 m	10000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[-2,5]	0.05 degree	[64,64]	N/A	Increase number of averages to improve data quality and range
2025-02-26	23:00	18 m	3600 m	10000	PPI	[180,180]	N/A	[193,283]	1 degree	RHI	[-2,5]	0.05 degree	[64,64]	N/A	Repeat PPI and RHI every 16 minutes, do stare scan at el=-0.4, az=64 in between
