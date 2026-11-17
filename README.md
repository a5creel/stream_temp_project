# Stream Temperature and Watershed Development in Connecticut

*Capstone project for the YSE Environmental Data Science Certificate.*
*Author: [Your Name] · Last updated: [Month Year]*

> **Demo note:** This is a worked example for Capstone Prompt 2. The data are made up.

## Project description

Paved surfaces such as roads, parking lots, and roofs heat up in the sun and send warm runoff into nearby streams. Warmer water holds less oxygen and stresses cold-water species like brook trout. This project uses monthly stream temperature records from monitoring stations across Connecticut to see whether streams draining more developed watersheds run warmer, especially in summer.

## Research question

**Do streams in more developed watersheds (a higher percentage of impervious surface) have warmer summer water temperatures?**

## Data

| File | Description | Source |
|------|-------------|--------|
| `raw_data/stream_temp_raw.csv` | Monthly mean water temperature (°C) at 7 stream monitoring stations, one column per month | [Name of agency / dataset, with link] — downloaded [date] |
| `raw_data/station_info_raw.csv` | One row per station: river name, % impervious surface in the upstream watershed, elevation (m), latitude, longitude | [Name of agency / dataset, with link] — downloaded [date] |
| `clean_data/stream_temp_clean.csv` | Cleaned, combined data: one row per station per month, with station info attached | Made by `scripts/1_data_clean.ipynb` |

## Repository structure

```
stream_temp_project/
├── README.md                    ← you are here
├── raw_data/                    ← original data, never edited
│   ├── stream_temp_raw.csv
│   └── station_info_raw.csv
├── clean_data/                  ← output of 1_data_clean.ipynb
│   └── stream_temp_clean.csv
├── figures/                     ← plots and tables (coming in later scripts)
└── scripts/                     ← notebooks, numbered in the order they run
    ├── 0_project_setup.ipynb
    └── 1_data_clean.ipynb
```

## How to run the code

All scripts are Google Colab notebooks written in R. Run them in number order:

1. `scripts/0_project_setup.ipynb` creates the project folder and subfolders in Google Drive.
2. `scripts/1_data_clean.ipynb` reads both raw files, reshapes and joins them, and writes `clean_data/stream_temp_clean.csv`.
3. `scripts/2_analysis.ipynb` *(coming soon)* will compare summer temperatures across watersheds and save figures to `figures/`.

Each notebook mounts Google Drive with one Python cell (`drive.mount()`), then switches to the R runtime. See the notes at the top of each notebook.

## Data cleaning steps (`1_data_clean.ipynb`)

1. **Reshaped the temperature data from wide to long** with `pivot_longer()`. The raw file has one column per month. After reshaping, each row is one station in one month, which makes it easy to filter, summarize, and plot by month.
2. **Added a month number** (`month_num`, 1–12) so months sort in calendar order instead of alphabetical order.
3. **Dropped 2 missing readings** (ST03 in February, ST05 in August).
4. **Matched the key column names.** The station file calls the ID `Station`, so I made all its column names lowercase and renamed `station` to `station_id`.
5. **Joined the station info onto the temperature data** with `left_join()` by `station_id`, so each temperature reading carries its watershed's % impervious surface.

**Known data issues:**
- **ST07** has temperature data but no station info, so its station columns are `NA`. It will be left out of analyses that need % impervious surface until the missing info is found.
- **ST08** (Eightmile River) has station info but no temperature data, so it isn't in the clean file.

## Status and next steps

- [x] Set up the project folder and GitHub repository
- [x] Clean and combine the raw data
- [ ] Exploratory plots: temperature by month for each station
- [ ] Compare July–August mean temperature against % impervious surface


## AI Disclosure 
I used Claude Opus 5.5 as an aid to write my README. The research question is my own. I found the datasets myself, and all data cleaning steps were determined on my own. I used Claude as a writing aid to organize the steps I completed on my own into this organized README. 
