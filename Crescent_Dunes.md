# Lidar Data

## Directory Structure

```
CD/lidar/raw_lidar_data/YYYYMM/YYYYMMDD
```

* **Background_DDMMYY-hhmmss.txt**
  Taken every hour by the lidar to remove any non-white features present in the noise floor.

* **User1_0006_YYYYMMDD_hhmmss.hpl**
  User-prescribed PPI (horizontal) scan.

* **User2_0006_YYYYMMDD_hhmmss.hpl**
  User-prescribed RHI (vertical) scan.

* **Stare_0006_YYYYMMDD_hh.hpl**
  Stare scan, conducted in between user-prescribed scans.

```
CD/lidar/processed_lidar_data
```

* **cd.lidar.z01.b0.YYYYMMDD.hhmmss.PPI.CD.PPI.nc**
  Standardized and quality-controlled PPI scan.

* **cd.lidar.z01.b0.YYYYMMDD.hhmmss.RHI.CD.RHI.nc**
  Standardized and quality-controlled RHI scan.

* **cd.lidar.z01.b0.YYYYMMDD.hhmmss.{scan}.CD.{scan}_wind_speed.png**
  Figures showing radial wind speed before and after filtering.

* **cd.lidar.z01.b0.YYYYMMDD.hhmmss.{scan}.CD.{scan}_probability.png**
  Probability distributions of measured points versus radial wind speed and SNR, following Beck & Kühn (2017).

* **cd.lidar.z01.b0.YYYYMMDD.hhmmss.{scan}.CD.{scan}_angles.png**
  Actual and regularized lidar beam angles during the scan.

---

## Quality Control Processing

Quality control is implemented using **LiDARGO**:
[https://github.com/StefanoWind/FIEXTA/tree/main/lidargo/lidargo](https://github.com/StefanoWind/FIEXTA/tree/main/lidargo/lidargo)

All parameters used to generate each quality-controlled NetCDF file are recorded in the attributes of the `qc_wind_speed` variable.

### Processing Steps

1. Read scan start time from file metadata and rename file using scan type and timestamp.
2. Read all lines from `.hpl` file and reformat into an `xarray.Dataset` with dimensions `time` and `range_gate`.
3. Add metadata from file header as global attributes (range gate length, number of gates, scan type, pulses per ray).
4. Calculate signal-to-noise ratio (SNR) and radial distance.
5. Bin azimuth and elevation values and update to bin centers within tolerance.
6. Remove data outside prescribed limits for range, radial wind speed, and SNR.
7. Apply dynamic filtering based on Beck & Kühn (2017).
8. Bin data in space and time.
9. Normalize radial wind speed and SNR within each bin.
10. Remove bins with insufficient samples.
11. Remove points with standard error exceeding the limit.
12. Compute a two-dimensional histogram of normalized radial wind speed and SNR.
13. Identify probability threshold where dispersion increases beyond a prescribed value.
14. Remove points below the probability threshold.
15. Remove bins without sufficient remaining data.
16. Re-index data into dimensions `[range, beamID, scanID]` based on scan type (PPI or RHI).
17. Add descriptive attributes and units to all variables and global attributes.
18. Generate and save figures documenting the quality control procedure.

---

## File Structure

### Raw Lidar Files

```
CrescentDunes_Lidar/raw_lidar_data/YYYYMM/YYYYMMDD/
  UserN_0006_YYYYMMDD_hhmmss.hpl
```

### Sample Header

```
Filename:        User1_0006_20250201_035307.hpl
System ID:       0006
Number of gates: 200
Range gate length (m): 18.0
Gate length (pts): 6
Pulses/ray:      10000
No. of rays in file: 91
Scan type:       User file 1 - stepped
Focus range:     65535
Start time:      20250201 03:53:13.78
Resolution (m/s): 0.0382
```

Range of measurement (center of gate):

```
(range gate + 0.5) * gate length
```

### Data Layout

**Data line 1** (beam information):

```
Decimal time (hours)
Azimuth (degrees)
Elevation (degrees)
Pitch (degrees)
Roll (degrees)
```

Format:

```
f9.6, 1x, f6.2, 1x, f6.2
```

**Data line 2** (per range gate):

```
Range Gate
Doppler (m/s)
Intensity (SNR + 1)
Beta (m-1 sr-1)
```

Format (repeated for each gate):

```
i3, 1x, f6.4, 1x, f8.6, 1x, e12.6
```

---

## Processed NetCDF Files

```
CrescentDunes_Lidar/processed_lidar_data/
  cd.lidar.z01.b0.YYYYMMDD.hhmmss.XXX.CD.XXX.nc
```

### Global Attributes

* Range gate length (m)
* Number of gates
* Pulses per ray
* Scan mode
* File history

### Dimensions

* `range`
* `beamID`
* `scanID` *(each file contains a single scan, so `scanID = 1`)*

### Variables

| Variable        | Dimensions              | Description                   | Units                  |
| --------------- | ----------------------- | ----------------------------- | ---------------------- |
| `x`             | (range, beamID, scanID) | Eastward distance             | m                      |
| `y`             | (range, beamID, scanID) | Northward distance            | m                      |
| `z`             | (range, beamID, scanID) | Vertical distance             | m                      |
| `wind_speed`    | (range, beamID, scanID) | Line-of-sight radial velocity | m/s                    |
| `SNR`           | (range, beamID, scanID) | Signal-to-noise ratio         | dB                     |
| `azimuth`       | (beamID, scanID)        | Beam azimuth angle            | °                      |
| `elevation`     | (beamID, scanID)        | Beam elevation angle          | °                      |
| `pitch`         | (range, beamID, scanID) | Lidar pitch angle             | °                      |
| `roll`          | (range, beamID, scanID) | Lidar roll angle              | °                      |
| `qc_wind_speed` | (range, beamID, scanID) | QC flag (0 = pass)            | –                      |
| `rws_norm`      | (range, beamID, scanID) | Normalized radial velocity    | m/s                    |
| `snr_norm`      | (range, beamID, scanID) | Normalized SNR                | dB                     |
| `probability`   | (range, beamID, scanID) | 2-D PDF value                 | –                      |
| `time`          | (beamID, scanID)        | Beam time (UTC)               | YYYY-MM-DD hh:mm:ss.ns |

QC flag values 1–11 are defined in the variable attributes along with all QC parameters.

---

## Lidar Settings and Scan Parameters

Lidar settings evolved throughout the campaign to balance data quality and scientific objectives.

| Date       | UTC Time | Range Gate | Measurement Range | Averages | Scan 1 | Elev. Range 1 | Elev. Res. 1 | Az. Range 1 | Az. Res. 1 | Scan 2 | Elev. Range 2 | Elev. Res. 2 | Az. Range 2 | Az. Res. 2 | Notes                                     |
| ---------- | -------- | ---------- | ----------------- | -------- | ------ | ------------- | ------------ | ----------- | ---------- | ------ | ------------- | ------------ | ----------- | ---------- | ----------------------------------------- |
| 2024-09-04 | 19:30    | 18 m       | 7200 m            | 3000     | PPI    | [0,0]         | N/A          | [0,90]      | 1°         | RHI    | [0,5]         | 0.1°         | [45,45]     | N/A        | Original scan pattern                     |
| 2024-09-05 | 17:00    | 18 m       | 7200 m            | 3000     | PPI    | [0,0]         | N/A          | [0,90]      | 1°         | RHI    | [0,5]         | 0.1°         | [45,45]     | N/A        | Changed time to UTC, removed gate overlap |
| 2024-09-12 | 16:30    | 18 m       | 7200 m            | 3000     | PPI    | [180,180]     | N/A          | [180,270]   | 1°         | RHI    | [0,5]         | 0.1°         | [45,45]     | N/A        | Flip PPI scan head                        |
| 2024-09-12 | 18:00    | 18 m       | 7200 m            | 3000     | PPI    | [180,180]     | N/A          | [180,270]   | 1°         | RHI    | [0,5]         | 0.1°         | [64,64]     | N/A        | Align RHI with wind                       |
| 2024-09-12 | 20:00    | 18 m       | 7200 m            | 3000     | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [0,5]         | 0.1°         | [64,64]     | N/A        | Exclude obstacle                          |
| 2024-09-19 | 22:30    | 12 m       | 3480 m            | 3000     | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [0,5]         | 0.1°         | [64,64]     | N/A        | Higher resolution                         |
| 2024-09-20 | 21:30    | 12 m       | 3480 m            | 3000     | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [0,5]         | 0.05°        | [64,64]     | N/A        | Increased RHI resolution                  |
| 2024-09-20 | 22:30    | 12 m       | 3480 m            | 3000     | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [-2,5]        | 0.05°        | [64,64]     | N/A        | Include negative elevation                |
| 2024-09-20 | 23:45    | 18 m       | 3600 m            | 3000     | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [-2,5]        | 0.05°        | [64,64]     | N/A        | Reduced resolution                        |
| 2024-12-05 | 19:00    | 18 m       | 3600 m            | 10000    | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [-2,5]        | 0.05°        | [64,64]     | N/A        | Increased averaging                       |
| 2025-02-26 | 23:00    | 18 m       | 3600 m            | 10000    | PPI    | [180,180]     | N/A          | [193,283]   | 1°         | RHI    | [-2,5]        | 0.05°        | [64,64]     | N/A        | Repeat scans every 16 min                 |
