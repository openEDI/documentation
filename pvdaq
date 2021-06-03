#  PV Durability and Relaibility Database (PVDAQ)

## Description

The PV Durability and Relaibility Database (PVDRDB) is built to support the aquisition and analysis of PV array time-series data. The database is built on the AWS RedShift framework. This framework is a hybrid of a PostgreSQL 8 deployment that has been converted to utilize columnar indexing. The database is deployed within the AWS Fedramp NREL cloud and is one piece of a systems that includes a series of tools to provide:

    * Automated importing of field data
    * API data harvesting for remote customers
    * A series of S3 repositories for ingenstion and archiving of data
    * Tools for direct access for data analysis
    * A Web application to allow external collaborator access, public data access, and adding or modifiying new and exisitng PV systems.

## Data Dictionary

Column | Type | Description

## Metadata

## Methods and Assumptions

### Data Sources
### Assumptions  

## Python Connection Examples

Athena data connection using PyAthena:
```python

import pandas as pd
from pyathena import connect

conn = connect(
    s3_staging_dir='s3://<user-defined>/<>', ##user defined staging directory
    region_name='us-west-2',
    work_group='<USER SPECIFIC WORKGROUP>'  ##specify workgroup if exists
)
```

Example #1: Querying with a limit:
```python
df = pd.read_sql("SELECT * FROM oedi.pvrdb_pr_developable_planes limit 8;", conn)
```

Example #2: Querying for a specific county name:
```python
df = pd.read_sql("SELECT * FROM oedi.pvrdb_pr_developable_planes WHERE county = 'San Juan' limit 8;", conn)
```

Example #3: Querying for a specific county FIPS (FIPS=127):
```python
df = pd.read_sql("SELECT * FROM oedi.pvrdb_pr_developable_planes WHERE geoid LIKE '72127%' limit 8;", conn)
```

For jupyter notebook example see our notebook which includes partitions and data dictionary:
[examples repository](https://github.com/openEDI/open-data-access-tools/tree/integration/examples)

## Related Links:

## Disclaimer and Attribution

Copyright (c) 2020, Alliance for Sustainable Energy LLC, All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
