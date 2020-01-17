# PV Rooftops

The PV Rooftops dataset is provided in Parquet format partitioned by city.  There are 4 core datasets stored in S3 partitioned by region(city)-year for downloads of single cities or to allow city-specific queries or queries across the dataset using Glue/Athena:

/aspects   

  `gid` bigint,    
  `city` string,    
  `state` string,    
  `year` bigint,    
  `bldg_fid` bigint,    
  `aspect` bigint,    
  `the_geom_96703` string,    
  `the_geom_4326` string,    
  `region_id` bigint,    
  `__index_level_0__` bigint   
  
/buildings   

  `gid` bigint,    
  `bldg_fid` bigint,    
  `the_geom_96703` string,    
  `the_geom_4326` string,    
  `city` string,    
  `state` string,    
  `year` bigint,    
  `region_id` bigint,    
  `__index_level_0__` bigint   
  
/developable_planes   

  `bldg_fid` bigint,    
  `footprint_m2` double,    
  `slope` bigint,    
  `flatarea_m2` double,    
  `slopeconversion` double,    
  `slopearea_m2` double,    
  `zip` string,    
  `zip_perc` double,    
  `aspect` bigint,    
  `gid` bigint,    
  `city` string,    
  `state` string,    
  `year` bigint,    
  `region_id` bigint,    
  `the_geom_96703` string,    
  `the_geom_4326` string,    
  `__index_level_0__` bigint   
  
/rasd   

  `gid` bigint,    
  `the_geom_96703` string,    
  `the_geom_4326` string,    
  `city` string,    
  `state` string,    
  `year` bigint,    
  `region_id` bigint,    
  `serial_id` bigint,    
  `__index_level_0__` bigint   



Within each core dataset there are paritions by city_state_year(YY) that can be queried directly via Athena or PrestoDB with relatively quick response times, or downloaded as a Parquet format data file. 

Aspects Lookup:   
1	337.5 - 22.5	north   
2	22.5 - 67.5	northeast   
3	67.5 - 112.5	east   
4	112.5 - 157.5 southeast    
5	157.5 - 202.5	south   
6	202.5 - 247.5	southwest   
7	247.5 - 292.5	west   
8	292.5 - 337.5	northwest   
0	flat	flat   
   
Regions Lookup:   
1	Albany	NY	2006-01-01      
2	Albany	NY	2013-01-01      
3	Albuquerque	NM	2006-01-01   
4	Albuquerque	NM	2012-01-01   
5	Allentown	PA	2006-01-01   
6	Amarillo	TX	2008-01-01   
7	Anaheim	CA	2010-01-01   
8	Arnold	MO	2006-01-01   
9	Atlanta	GA	2008-01-01   
10	Atlanta	GA	2013-01-01   
11	Augusta	GA	2010-01-01   
12	Augusta	ME	2008-01-01   
13	Austin	TX	2006-01-01   
14	Austin	TX	2012-01-01   
15	Bakersfield	CA	2010-01-01   
16	Baltimore	MD	2008-01-01   
17	Baltimore	MD	2013-01-01   
18	Baton Rouge	LA	2006-01-01   
19	Baton Rouge	LA	2012-01-01   
20	Birmingham	AL	2008-01-01   
21	Bismarck	ND	2008-01-01   
22	Boise	ID	2007-01-01   
23	Boise	ID	2013-01-01   
24	Boulder	CO	2014-01-01   
25	Bridgeport	CT	2006-01-01   
26	Bridgeport	CT	2013-01-01   
27	Buffalo	NY	2008-01-01   
28	Carson City	NV	2009-01-01   
29	Charleston	SC	2010-01-01   
30	Charleston	WV	2009-01-01   
31	Charlotte	NC	2006-01-01   
32	Charlotte	NC	2012-01-01   
33	Cheyenne	WY	2008-01-01   
34	Chicago	IL	2008-01-01   
35	Chicago	IL	2012-01-01   
36	Cincinnati	OH	2010-01-01   
37	Cleveland	OH	2012-01-01   
38	Colorado Springs	CO	2006-01-01   
39	Colorado Springs	CO	2013-01-01   
40	Columbia	SC	2009-01-01   
41	Columbus	GA	2009-01-01   
42	Columbus	OH	2006-01-01      
43	Columbus	OH	2012-01-01   
44	Concord	NH	2009-01-01   
45	Corpus Christi	TX	2012-01-01   
46	Dayton	OH	2006-01-01   
47	Dayton	OH	2012-01-01   
48	Denver	CO	2012-01-01   
49	Des Moines	IA	2010-01-01   
50	Detroit	MI	2012-01-01   
51	Dover	DE	2009-01-01   
52	El Paso	TX	2007-01-01   
53	Flint	MI	2009-01-01   
54	Fort Wayne	IN	2008-01-01   
55	Frankfort	KY	2012-01-01   
56	Fresno	CA	2006-01-01   
57	Fresno	CA	2013-01-01   
58	Ft Belvoir	DC	2012-01-01   
59	Grand Rapids	MI	2013-01-01   
60	Greensboro	NC	2009-01-01   
61	Harrisburg	PA	2009-01-01   
62	Hartford	CT	2006-01-01   
63	Hartford	CT	2013-01-01   
64	Helena	MT	2007-01-01   
65	Helena	MT	2013-01-01      
66	Houston	TX	2010-01-01      
67	Huntsville	AL	2009-01-01   
68	Indianapolis	IN	2006-01-01   
69	Indianapolis	IN	2012-01-01   
70	Jackson	MS	2007-01-01   
71	Jacksonville	FL	2010-01-01   
72	Jefferson City	MO	2008-01-01   
73	Kansas City	MO	2010-01-01   
74	Kansas City	MO	2012-01-01   
75	LaGuardia JFK	NY	2012-01-01   
76	Lancaster	PA	2010-01-01      
77	Lansing	MI	2007-01-01   
78	Lansing	MI	2013-01-01   
79	Las Vegas	NV	2009-01-01   
80	Lexington	KY	2012-01-01   
81	Lincoln	NE	2008-01-01   
82	Little Rock	AR	2008-01-01   
83	Los Angeles	CA	2007-01-01   
84	Louisville	KY	2006-01-01   
85	Louisville	KY	2012-01-01   
86	Lubbock	TX	2008-01-01   
87	Madison	WI	2010-01-01   
88	Manhattan	NY	2007-01-01   
89	McAllen	TX	2008-01-01   
90	Miami	FL	2009-01-01   
91	Milwaukee	WI	2007-01-01   
92	Milwaukee	WI	2013-01-01   
93	Minneapolis	MN	2007-01-01   
94	Minneapolis	MN	2012-01-01   
95	Mission Viejo	CA	2013-01-01   
96	Mobile	AL	2010-01-01   
97	Modesto	CA	2010-01-01   
98	Montgomery	AL	2007-01-01   
99	Montpelier	VT	2009-01-01   
100	Newark	NJ	2007-01-01   
101	New Haven	CT	2007-01-01   
102	New Haven	CT	2013-01-01   
103	New Orleans	LA	2008-01-01   
104	New Orleans	LA	2012-01-01   
105	New York	NY	2005-01-01   
106	New York	NY	2013-01-01   
107	Norfolk	VA	2007-01-01   
108	Oklahoma City	OK	2007-01-01   
109	Oklahoma City	OK	2013-01-01   
110	Olympia	WA	2010-01-01   
111	Omaha	NE	2007-01-01   
112	Omaha	NE	2013-01-01   
113	Orlando	FL	2009-01-01   
114	Oxnard	CA	2010-01-01   
115	Palm Bay	FL	2010-01-01   
116	Pensacola	FL	2009-01-01   
117	Philadelphia	PA	2007-01-01   
118	Pierre	SD	2008-01-01   
119	Pittsburgh	PA	2004-01-01   
120	Pittsburgh	PA	2012-01-01   
121	Portland	OR	2012-01-01   
122	Poughkeepsie	NY	2012-01-01   
123	Providence	RI	2004-01-01   
124	Providence	RI	2012-01-01   
125	Raleigh-Durham	NC	2010-01-01   
126	Reno	NV	2007-01-01   
127	Richmond	VA	2008-01-01   
128	Richmond	VA	2013-01-01   
129	Rochester	NY	2008-01-01   
130	Rochester	NY	2014-01-01   
131	Sacramento	CA	2012-01-01   
132	Salem	OR	2008-01-01   
133	Salt Lake City	UT	2012-01-01   
134	San Antonio	TX	2008-01-01   
135	San Antonio	TX	2013-01-01   
137	San Diego	CA	2008-01-01   
138	San Diego	CA	2013-01-01   
139	San Francisco	CA	2013-01-01   
140	Santa Fe	NM	2009-01-01   
141	Sarasota	FL	2009-01-01   
142	Scranton	PA	2008-01-01   
143	Seattle	WA	2011-01-01   
144	Shreveport	LA	2008-01-01   
145	Spokane	WA	2008-01-01   
146	Springfield	IL	2009-01-01   
147	Springfield	MA	2007-01-01   
148	Springfield	MA	2013-01-01   
149	St Louis	MO	2008-01-01   
150	St Louis	MO	2013-01-01   
151	Stockton	CA	2010-01-01   
152	Syracuse	NY	2008-01-01   
153	Tallahassee	FL	2009-01-01   
154	Tampa	FL	2008-01-01   
155	Toledo	OH	2006-01-01   
156	Toledo	OH	2012-01-01   
157	Topeka	KS	2008-01-01   
158	Trenton	NJ	2008-01-01   
159	Tucson	AZ	2007-01-01   
160	Tulsa	OK	2008-01-01   
161	Washington	DC	2009-01-01   
162	Washington	DC	2012-01-01   
163	Wichita	KS	2012-01-01   
164	Winston-Salem	NC	2009-01-01   
165	Worcester	MA	2009-01-01   
166	Youngstown	OH	2008-01-01   
167	Andrews AFB	DC	2012-01-01   
136	San Bernardino-Riverside	CA	2012-01-01   
168	Tampa	FL	2013-01-01   

