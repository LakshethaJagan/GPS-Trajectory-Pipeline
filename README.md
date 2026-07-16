# GPS-Trajectory-Pipeline
End-to-end GPS trajectory cleaning, UKF smoothing, map matching, and bus fleet compliance monitoring pipeline built on real Teltonika FMB data — IIT Madras internship project.
# GPS Trajectory Analysis & Bus Fleet Monitoring System

A complete end-to-end geospatial data engineering pipeline that processes 
raw GPS telemetry from Teltonika FMB tracking devices, reconstructs clean 
vehicle trajectories, and monitors bus fleet compliance against authorised 
depot and stop locations.

Built during a Data Science and Machine Learning internship at IIT Madras 
under the MBRDI project — Transportation and Mobility Department.
---
## What this project does

- Cleans and filters raw GPS data from Teltonika FMB devices (handles 
  quoted CSV format, zero-satellite rows, GPS jumps, stationary duplicates)
- Classifies vehicle state using IO sensor ground truth (ignition, movement 
  sensor, odometer) instead of guessing from speed alone
- Smooths GPS trajectories using a custom Unscented Kalman Filter (UKF) 
  implementation — handles non-linear motion at curves better than standard KF
- Segments daily recordings into individual trips using ignition events
- Detects every rest period with exact start time, end time, duration, and 
  location — flags whether engine was on or off
- Performs HMM-based map matching to snap GPS points to actual road segments 
  using OpenStreetMap
- Checks fleet-wide compliance — determines if buses rest only at authorised 
  depots and bus stops, flags unauthorised stops on an interactive map
- Generates 5 interactive Folium maps, analytics charts, and CSV reports 
  per file
- Serves a 6-page Streamlit dashboard for fleet monitoring
---
## Tech Stack

Python · pandas · NumPy · SciPy
Custom UKF (from scratch, no external filter library)
Folium · OSMnx · NetworkX · leuvenmapmatching
Streamlit · Plotly · Matplotlib · openpyxl
---

## Dataset

- Device: Teltonika FMB GPS Tracker
- Region: Tirunelveli district, Tamil Nadu, India
- Coverage: 11 days of single-bus data + 1-day fleet data (28 buses)
- Raw records: ~18,500 per day per bus
- Fleet registry: 45 buses with vehicle numbers, route numbers, and IMEIs

---

## Key Results

| Metric | Value |
|--------|-------|
| Noise removed per day | 67% of raw records |
| Driving points preserved | 100% |
| GPS jitter reduction (UKF) | 62.2% |
| Daily distance (consistent) | 325–340 km |
| Fleet compliance (28 buses) | 64.7% at known stops |
| Unauthorised rest events | 35.3% across fleet |

---

## Project Structure

GPS_Universal_Framework/
├── data/
│   ├── raw/          ← place your CSV files here
│   ├── cleaned/      ← auto-generated cleaned CSVs
│   └── matched/      ← map matched route outputs
├── src/
│   ├── pipeline_core.py          ← load, clean, UKF, trips
│   ├── analytics/
│   │   ├── visualize.py          ← 5 maps + 2 charts
│   │   └── bus_stop_compliance.py← compliance engine
│   └── dashboard/
│       └── app.py                ← Streamlit dashboard
├── reports/          ← CSV reports per file
├── output/           ← HTML maps per file
├── config.py         ← only file you edit for new datasets
├── run_pipeline.py   ← single entry point
└── requirements.txt
---

## IO Sensor Integration (Teltonika specific)

| Column | Meaning | Used for |
|--------|---------|----------|
| IO_239 | Ignition (0=off, 1=on) | Trip start/end detection |
| IO_240 | Movement sensor (0=still, 1=moving) | Vehicle state classification |
| IO_16  | Odometer (cumulative metres) | Exact distance calculation |

---

## Vehicle States

| State | IO_239 | IO_240 | Meaning |
|-------|--------|--------|---------|
| ENGINE_ON_MOVING | 1 | 1 | Actively driving |
| ENGINE_ON_IDLING | 1 | 0 | Engine on, not moving |
| ENGINE_OFF_PARKED | 0 | 0 | Fully parked |
| ENGINE_OFF_MOVING | 0 | 1 | Anomaly — removed |

---

## Internship Details

- **Institution:** IIT Madras
- **Department:** Transportation and Mobility
- **Project:** MBRDI (Mercedes-Benz Research and Development India)
- **Period:** April 2026 – June 2026
- **Intern:** Lakshetha, B.E. CSE, Chennai Institute of Technology (2024–2028)

---

## License

This project was developed as part of an academic internship.  
For usage permissions contact [lakshethajagan1@gmail.com]
