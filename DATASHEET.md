# Dataset Documentation: Socioeconomic Health Disparities in Chicago

**Course:** UIC CS 418, Fall 2026  
**Project:** Socioeconomic-Health-Disparities  
**Checkpoint:** Four, Data Documentation  
**Group members:** Anirudh Gupta, Claudia Jimenez, Andrew Mcclendon, Auon Muhammad Naqvi, Jesus Rodriguez  
**Status:** **DRAFT FOR GROUP INTEGRATION**. Andrew's Chicago Community Population research is entered below, based on the group's CSV and EDA notebook. Earlier Open Air publisher research is preserved for the team, but should be reviewed by its current owners. Remaining contributors must complete their assigned areas and everyone must review before submission.

## Overview and dataset inventory

Our research examines how median household income and other community characteristics relate to air-pollution measurements across Chicago community areas. Our two **primary** datasets are Chicago Community Population and Open Air Chicago Day Aggregations. Median income and local emissions datasets may also be used to support analyses; their role and preprocessing must be documented where used.

| Dataset                                                                                                                                                                                      | Role               | Source                                 | Observation unit                                            | Working owner(s)                    |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | -------------------------------------- | ----------------------------------------------------------- | ----------------------------------- |
| [Open Air Chicago Day Aggregations](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data)                                             | Primary            | Chicago Data Portal                    | Sensor-day                                                  | Claudia; Auon (confirm assignments) |
| [Chicago Community Population / Chicago Health Atlas](https://chicagohealthatlas.org/indicators/POP)                                                                                         | Primary            | Chicago Health Atlas custom CSV export | One Chicago community area; ACS reference year labeled 2023 | Andrew ; Jesus                      |
| [ACS 5-Year Data by Community Area, Most Recent Year](https://data.cityofchicago.org/Community-Economic-Development/ACS-5-Year-Data-by-Community-Area-Most-Recent-Year/7umk-8dtw/about_data) | Secondary, if used | Chicago Data Portal                    | One Chicago community area (GEOID 1-77)                                                      | Anirudh / group                     |
| [Community Areas Local Emission 2019](https://datahub.cmap.illinois.gov/datasets/communityareas-localemission2019/about)                                                                     | Secondary, if used | CMAP                                   | One Chicago community area                                                      | Anirudh / group                     |

---

# Part A. Open Air Chicago Day Aggregations

**Section owners:** Claudia (Open Air EDA, as labeled in the group notebook); Jesus (methods and additional documentation). Andrew previously compiled background research retained here; group should confirm final sign-off.  
**Source:** [Chicago Data Portal dataset `rtmx-vkjr`](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data).  
**Snapshot analyzed:** API retrieval **October 27, 2026**. Do not mix this with the earlier EDA snapshot.

## A1. Motivation

Open Air Chicago was created to provide air-quality information to Chicago neighborhoods and support community-based environmental monitoring and environmental-justice assessment. The network is jointly managed and maintained by the **Chicago Department of Public Health (CDPH)** and the **University of Illinois Chicago (UIC) School of Public Health**. Its measurements represent conditions near outdoor monitoring locations rather than the direct exposures of individual residents. The dataset provides  air quality data that is more localized than the federal air quality index readings, including PM2.5 and NO2 readings.

The project has received support through two grants to Dr. Serap Erdal, UIC School of Public Health:

1. **Community-Driven Air Quality and Environmental Justice Assessment**. Congressional Earmark Grant / National Institute of Standards and Technology (NIST), **Grant No. 60NANB23D206**, **October 1, 2023 to September 30, 2026**.
2. **ComEd Hyperlocal Air Quality Assessment**. **RHP Risk Management / ComEd / Exelon, LLC**, **January 16, 2025 to January 31, 2026**.

Collaborators supporting development of the network include CDOT, Clarity Movement, ComEd/Exelon, Cook County Department of Environment and Sustainability, Illinois EPA, RHP Risk Management, UIC School of Public Health, and community organizations. This partner list should not be conflated with the two organizations specifically identified as maintainers.

## A2. Composition

Each row describes a **single sensor over a complete UTC day**. The original October 27 API extract contains **121,606 rows and 46 columns**. The group's cleaned analysis extract selects 15 columns and removes any rows with missing values in those selected columns, yielding **119,859 rows and 15 columns**. Thus, **1,747 rows (about 1.44%)** were excluded and **98.56%** of records were retained.

The cleaned snapshot spans **July 26, 2025 to October 26, 2026**, with **zero unparsed observation dates**. Relevant content includes sensor identifiers, date, geographic coordinates, PM2.5 mass measurements, NO2 concentrations, and raw or cloud-calibrated measurement versions. Measurements are aggregated daily by mean. According to the Chicago Data Portal website, “24-hour mean measurements are calculated by averaging all measurements collected over a complete UTC day. For example, the 24-hour mean for 2026-01-01 aggregates all measurements from 00:00 UTC to 23:59:59 UTC on 2026-01-01.”

**Missingness/quality detail:** The older EDA notebook shows that the black-carbon variables were completely missing in that earlier extract; black carbon is not one of the 15 selected analysis fields. Across the October 27 snapshot (121,606 rows), filtering the 15 selected columns dropped exactly 1,747 rows (1.44%) due to missing sensor observations during maintenance and calibration, retaining 119,859 rows (98.56%). A check on `(datasourceid, startofperiod)` confirmed zero duplicate records, and unselected columns were omitted because they were over 99.9% blank.

## A3. Collection process

The Open Air Chicago network deployed **277 sensors across all 77 Chicago community areas**. Additional sensors installed at federal monitoring sites support the development of a potential local correction factor. Therefore, 277 deployed network sensors is not necessarily the same as the count of unique sensor identifiers in the API file. Measurements come from the Clarity monitoring network and undergo automated quality-control checks and cloud calibration before daily aggregation.

Daily aggregates are reported for complete **UTC days**. The dataset's geographic positions are monitoring sites rather than sampled individuals or residences, and sensor reporting can vary over time.

**Additional methods:** Sensors are Clarity Node-S boxes mounted about 10 to 15 feet up on city utility poles. Automated cloud software adjusts the data for weather and sensor drift using EPA comparison monitors at Mayfair, Schiller Park, and Washington High School. Particulate matter (PM2.5) is measured in micrograms per cubic meter, and nitrogen dioxide (NO2) is measured in parts per billion.

## A4. Preprocessing, cleaning and labeling

The group's October 27 analysis selects **15 fields** from the 46-column API snapshot and filters using `drop_nulls()` across the selected fields. This reduces the source from **121,606** to **119,859** sensor-day observations. The raw data is available, and raw data for some readings, including PM2.5 and NO2, are cloud-calibrated with a correction factor applied. We plan to use **cloud-calibrated `.value` measurements** and **PM2.5 mass concentrations** for pollution analysis. Date parsing of the cleaned snapshot returned zero unparsed timestamps.

**Reproduction and important limitations:** We cleaned the data in `Jesus_Data.ipynb` and `eda.ipynb` using Python with Polars and Pandas. We pulled the raw CSV from the Chicago Data Portal API. Because the API cuts off long column names, the PM2.5 AQI columns appear as `pm2_5concmass24hourmeanu` (raw) and `pm2_5concmass24hourmeanu_1` (calibrated). Some NO2 readings had small negative numbers (around -0.5 to 0.0) due to sensor noise, so we set those to 0.0 instead of throwing out the entire row. We pulled neighborhood names out of `sensor_name` by removing trailing numbers, and we changed "O'HARE" to "OHARE" so it matches the other city datasets. Collocation sensors at EPA stations were kept separate so they don't distort normal neighborhood averages.

## A5. Uses

The dataset supports descriptive comparisons of local pollution measurements and changes across days or locations, including community-area summaries that can be compared with socioeconomic data after a verified geographic join. It should **not** be used as a direct estimate of any resident's personal exposure or as standalone proof of regulatory compliance. Daily aggregation can hide short-duration peaks, location coverage may be uneven, and observed income/pollution associations must not be described as causal without appropriate study design.

## A6. Distribution

The dataset is publicly available from the [City of Chicago Data Portal](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data) as a **CSV download** and through a **Socrata API** such as `https://data.cityofchicago.org/resource/rtmx-vkjr.csv`. The team member verified permission for downloading, analysis, processing and redistribution.

License/terms of use: no named open-data license found. The City of Chicago's standard data terms of use apply. No specific attribution is required beyond noting the data is modified from cityofchicago.org and provided as-is with no accuracy guarantee. Full disclaimer: [https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html]

## A7. Maintenance and evolution

The network is jointly managed and maintained by **Chicago Department of Public Health (CDPH) and UIC School of Public Health**. The City Data Portal describes **daily updates reflecting previous-day statistics**. The analyzed October 27, 2026 download had observations through October 26, 2026. As an evolving resource, dataset row counts will change with later retrievals, so the analysis should identify and preserve a consistent data snapshot.

---

# Part B. Chicago Community Population / Chicago Health Atlas

**Research contributor:** Andrew Mcclendon (Population dataset owner in `EDA-Files/eda.ipynb`). **Group reviewers / methods support:** [TODO: confirm].  
**Data file used by the group:** `data/Chicago_Community_Population_Data.csv`.  
**Publisher:** [Chicago Health Atlas](https://chicagohealthatlas.org/), a platform developed by the Chicago Department of Public Health (CDPH), UIC's PHAME Center and Metopio.  
**Relevant indicator documentation:** [Population (POP)](https://chicagohealthatlas.org/indicators/POP), [Demographics (PCT)](https://chicagohealthatlas.org/indicators/PCT), [Median household income (INC)](https://chicagohealthatlas.org/indicators/INC). These pages document related indicators; the group's **custom 30-column export is not identical** to any single one of these pages.  
**Source version:** The CSV labels its records `ACS Year = 2023`. The exact original export date, release version and ACS aggregation window were not independently verified. The file analyzed here is the group's saved CSV, not a new 2019–2023 download.

## B1. Motivation

The Chicago Health Atlas was created to make Chicago's health and demographic information accessible, transparent and comparable across communities, and to help residents and researchers better understand differences in local conditions and health equity. The platform was developed by CDPH, the UIC PHAME Center and Metopio. The original demographic figures are sourced from the **U.S. Census Bureau**, including the American Community Survey (ACS), rather than being direct observations collected by the research group.

**Why our group selected this dataset:** We are studying how community-area demographic and socioeconomic characteristics relate to variation in outdoor air pollution across Chicago. The data provides context about population composition and distributions of households among income brackets. The goal is a **community-level comparison**, not prediction of any individual resident's income or pollution exposure.

**Funding:** The Chicago Health Atlas homepage lists support from the Otho S. A. Sprague Memorial Institute and others for the platform. We have **not** verified a dataset-specific grant or funding source for the exported demographic observations, so no such grant is attributed to this CSV.

## B2. Composition

The group file has **77 rows and 30 columns**, one row per **Chicago community area**, with **77 distinct community-area names**. All `ACS Year` entries are **2023**. It contains **no null or blank cells**, and no duplicated `Record ID` values in the uploaded snapshot. Counts are encoded in the CSV as strings with commas in many columns and should be cast to numeric types before calculations.

Columns include five household-income **brackets (counts of households)**; 12 age-by-sex categories (**resident counts**); `Total Population`; seven race-count categories plus `White Not Hispanic or Latino` and `Hispanic or Latino`; and identifiers (`ACS Year`, `Community Area`, `Record ID`). These categories need careful interpretation: ethnicity can overlap race, so race and Hispanic origin should **not** automatically be summed together.

**Basic integrity check:** The age-by-sex counts differ from `Total Population` by 1–2 people in **50 of 77** community areas. The seven general race-count columns differ from `Total Population` by 1–2 people in **37 of 77** community areas. These are small discrepancies, but their cause is **not established**. Treat them as a consistency note rather than assuming exact additive equality or inventing a reason.

**Key geographic sample:** 77 Chicago community areas. **Time label:** `ACS Year = 2023`; the exact underlying ACS multi-year window for this custom export is **unconfirmed**.

## B3. Collection process

The Population (POP) indicator identifies the U.S. Census Bureau's **American Community Survey** as the original source for population estimates outside decennial Census years, with **Table B01001** for sex and age and **Table B03002** for race/ethnicity. The same POP documentation identifies the **Decennial Census (Table P012)** for decennial years such as 2020; it should not be described as the immediate source of the group's 2023-labeled estimates without further evidence. The Demographics (PCT) page also discusses ACS sampling and demographic ratios. Income-related fields were selected in the Atlas's export interface; the exact ACS table(s) for **each household-income bracket** have not been independently mapped.

The ACS surveys a sample of U.S. households over time; multi-year pooled estimates support smaller geographies. Unlike a full population census, ACS values are **estimates** subject to sampling variation and nonresponse. The group's CSV was generated through the **Chicago Health Atlas download tool** by selecting population, demographic and household-income items for Chicago community areas. It is **not** an untouched one-indicator POP download.

**Reporting-window caveat:** The original file labels 2023, but that alone does not identify a five-year period. For example, a separate verified download of the Atlas POP indicator for **2019–2023** showed **46,804** residents for Albany Park, whereas the saved 30-column group CSV reports **47,830**. Therefore, we do **not** claim the group's original export uses exactly the same release, window or population definition as that separate download.

## B4. Preprocessing, cleaning and labeling

The original 77-row CSV was **not documented as having rows removed**. In `EDA-Files/eda.ipynb`, the group:

1. Loaded `data/Chicago_Community_Population_Data.csv` into Polars as `pop` and inspected shape, schema, missing values and descriptive statistics.
2. Identified the five household-income bracket columns; when stored as strings, removed comma separators and cast them to 64-bit integers for calculations.
3. Generated a box plot comparing counts of households across the five income brackets.
4. Calculated a derived `Total Households` as the row-wise sum of those five bracket counts.
5. Created five derived **percentage-of-households** columns by dividing each bracket by `Total Households` and multiplying by 100.
6. Sorted community areas by the derived share in the `$125,000 +` bracket, then produced a stacked percentage bar chart.

These transformations were **in-memory exploratory steps**, not evidence that the raw saved CSV was permanently changed. The notebook shows **no drop-null or row-filter step for `pop`**. Its documented population EDA therefore retains the 77 input community areas, prior to any later cross-dataset matching. The chart code is not itself a final joined analytic dataset.

**Open checks:** [TODO group: validate household category definitions, confirm numeric type conversion for any additional columns used in modeling, and report before/after row counts if any *subsequent* joins or filters are applied.]

## B5. Uses

The intended use is to describe and compare Chicago community-area demographics, especially household-income distributions and population composition, alongside air-pollution statistics. Group-level patterns may help motivate research about environmental inequality and disparities.

**Inappropriate interpretations:** This dataset does not reveal any person's income, demographic identity, individual pollution exposure or health outcome. A correlation between area-level demographics and pollution does **not** establish causation. A single community-area summary can also conceal substantial variation within that area. Comparisons across datasets should disclose the difference between the **2023-labeled** population file and the **2025–2026** air monitoring data.

## B6. Distribution

The group exported a **CSV** through [Chicago Health Atlas](https://chicagohealthatlas.org/). Public downloading and use for analysis are supported by the site's tools and purpose. The researcher reported verifying permission for downloading, processing and redistribution. However, no **single named license** applying to the **whole custom export** was identified in the reviewed source documentation. The Atlas contains indicators originating from multiple providers, and permission to download should not be equated with an unrestricted blanket license for every possible use.

**Reuse/attribution approach:** Credit **Chicago Health Atlas** for the assembled export and the **U.S. Census Bureau / ACS** for underlying demographic data; preserve the selected indicators, file and reference-year metadata; and check the applicable platform/source conditions before publishing the entire compiled export elsewhere. Do not transfer restrictions from unrelated websites as if they were Chicago Health Atlas policies.

## B7. Maintenance and evolution

The Chicago Health Atlas homepage describes the platform as an **evolving resource** with continuous additions and updates. It was developed by CDPH, UIC PHAME Center and Metopio. The **dataset-specific update frequency and original export date of the group's 30-column file were not verified**. The `ACS Year = 2023` column is a data reference label, **not** proof of when the Atlas last refreshed this file. A reproducible analysis should retain the saved group CSV and document its reference year, rather than re-download a changing combination of indicators without recording settings.

---

# Part C. Data scope: population, access frame and actual samples

| Question                       | Open Air Chicago                                                                                                                                          | Community Population (Andrew)                                                                                                                                                             |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Target population**          | Chicago's 77 community areas; residents themselves are not measured. | Residents and households across **all 77 Chicago community areas**, as represented in area-level published estimates; no direct person-level records.                                     |
| **Access / observation frame** | Sensors reporting observations through the monitoring network, including additional federal-site monitors.                                  | 77 published community-area summaries in the **saved Chicago Health Atlas custom export**, with indicators selected using the download tool.                                              |
| **Actual analysis sample**     | Sensors that are functioning properly and returning readings during the specified timeframe.                                    | **77 rows x 30 columns** in the supplied CSV, with **0 missing or blank cells**; EDA derives household measures in memory.                                                                |
| **Geographic coverage**        | Network deployed in all 77 Chicago community areas, with unequal site-level coverage possible.                                                            | One row per Chicago community area; community-area aggregates are not individual residence observations.                                                                                  |
| **Temporal coverage**          | Cleaned observations 2025-07-26 to 2026-10-26; not proof of uninterrupted coverage at each site.                                                          | CSV's `ACS Year` is **2023**; **exact original ACS release and underlying multi-year period are unverified**.                                                                             |
| **Bias / exclusions**          | Calibration noise.             | ACS sample/nonresponse uncertainty, changes across years, neighborhood-level aggregation, within-area variation, source/period mismatch and minor subtotal inconsistencies (1–2 persons). |

Coverage, grain, and year differences: Open Air updates daily and spans July 2025 to October 2026. The secondary datasets don't, Median Income is a 2020-2024 average and Local Emission is frozen at 2019. So pollution readings get compared against income/emissions numbers from a different time window, which is worth keeping in mind when trying to discern any trend.

All three datasets join at the same level, one row per community area, 77 total. Income joined to Local Emission matched 75 of 77 areas (2 dropped from naming mismatches like apostrophes). Open Air joined to the secondary data by standardized community name, match counts are printed in Jesus_Data.ipynb.

---

# Part D. Codebooks and variable definitions

## D1. Open Air Chicago codebook (Jesus and Claudia)

Here are the selected variables kept for our analysis from the Open Air Chicago dataset, with descriptions for each column provided by the Chicago Data Portal. Raw vs cloud-calibrated values and EPA AQI readings are included for possible comparison.

Note: PM2.5 is measured in mass and number of particles. Mass is the gold standard for many health and air quality measurements, and the number of particles can be more difficult to interpret. However, both columns are currently kept for comparison.

* "datasourceid": Sensor identification number
* "startofperiod": Start of time period, in UTC (not Chicago time)
* "sensor_name": Community area name and number of sensor in community area
* "no2Conc24HourMean.raw": NO2 concentration, 24-hour mean (ppb), raw
* "no2Conc24HourMean.value": NO2 concentration, 24-hour mean (ppb), value
* "pm2_5ConcMass24HourMean.raw": PM2.5 mass concentration, 24-hour mean (μg/m³), raw
* "pm2_5ConcMass24HourMean.value": PM2.5 mass concentration, 24-hour mean (μg/m³), value
* "pm2_5ConcMass24HourMeanUsEpaAqi.raw": PM2.5 mass concentration, 24-hour mean, UsEpaAqi, raw
* "pm2_5ConcMass24HourMeanUsEpaAqi.value": PM2.5 mass concentration, 24-hour mean, UsEpaAqi, value
* "pm2_5ConcNum24HourMean.raw": PM2.5 number concentration, 24-hour mean (#/cm³), raw
* "pm2_5ConcNum24HourMean.value": PM2.5 number concentration, 24-hour mean (#/cm³), value
* "latitude": Latitude of the sensor location
* "longitude": Longitude of the sensor location
* "location": Location of the sensor in a format that allows for creation of maps and other geographic operations on this data portal
* "record_id": A unique ID for the record

Missing values in any of these selected columns were filtered out entirely by dropping rows.

## D2. Chicago Community Population codebook (Andrew, initial inventory)

The entries below refer to the **saved 30-column group CSV**. Comma-formatted number fields appear as text in the CSV and require numeric parsing. Exact indicator-specific ACS tables and suppression/rounding rules are **not established for every field**.

| CSV field(s)                                                                                                                                          | Field meaning                                                | Expected unit / type          | Notes                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | ----------------------------- | ------------------------------------------------------------------------------ |
| `ACS Year`                                                                                                                                            | ACS reference year recorded by export                        | Year label (2023)             | Not proof of exact multi-year window                                           |
| `Community Area`                                                                                                                                      | Chicago community-area name                                  | Text                          | Distinct in each of 77 rows; join key needs normalization/validation           |
| `Under $25,000`; `$25,000 to $49,999`; `$50,000 to $74,999`; `$75,000 to $125,000`; `$125,000 +`                                                      | Counts of households in five reported annual-income brackets | Households, integer-like text | Original bin labels retained; exact technical definitions unverified           |
| `Male 0 to 17`; `Male 18 to 24`; `Male 25 to 34`; `Male 35 to 49`; `Male 50 to 64`; `Male 65+`                                                        | Male counts by age group                                     | Residents, integer-like text  | Age groups as published                                                        |
| `Female 0 to 17`; `Female 18 to 24`; `Female 25 to 34`; `Female 35 to 49`; `Female 50 to 64`; `Female 65 +`                                           | Female counts by age group                                   | Residents, integer-like text  | Preserve exact `Female 65 +` field spelling                                    |
| `Total Population`                                                                                                                                    | Community-area population estimate                           | Residents, integer-like text  | Small 1–2 resident discrepancies with category sums                            |
| `White`; `Black or African American`; `American Indian or Alaska Native`; `Asian`; `Native Hawaiian or Pacific Islander`; `Other Race`; `Multiracial` | Published race counts                                        | Residents, integer-like text  | Individual race counts are not ethnicity counts                                |
| `White Not Hispanic or Latino`; `Hispanic or Latino`                                                                                                  | Additional race/ethnicity categories                         | Residents, integer-like text  | Race and Hispanic ethnicity categories may overlap; do not sum all as disjoint |
| `Record ID`                                                                                                                                           | Export row identifier                                        | Text                          | Example pattern `2023_ALBANY PARK`; unique in supplied file                    |

**Observed quality checks:** 77 unique community areas, 77 unique record IDs, no blank/null cells. Age/sex subtotals differ from `Total Population` by at most two in 50 community areas; the seven race-count subtotals differ by at most two in 37 community areas. Cause and any source rounding convention remain unverified.

**For group review:** [TODO: verify the Atlas field definitions and the exact underlying income-bracket indicator IDs/tables before calling this a complete publisher-validated codebook.]

## D3. Secondary-dataset variables used in joins (Anirudh / group)

Median Income (Chicago_Median_Income_By_Community_Area.csv), 2020-2024:
| Column | Meaning | Unit |
|---|---|---|
| Name | Community area name | text |
| GEOID | Community area number (1-77) | id |
| INC_2020-2024 | Median household income | USD |
| PCI_2020-2024 | Per capita income | USD |

Local Emission (CommunityAreas_LocalEmission2019.csv), 2019:
| Column | Meaning | Unit |
|---|---|---|
| COMMUNITY | Community area name | text |
| POP | Population | count |
| RES_ELE_CO, NONRES_ELE, RES_NG_CO2, NONRES_NG_, ONROAD_CO2, WASTE_CO2E | CO2-equivalent emissions by source (summed to total_co2) | tons/year |

Both used in Ani_Data.ipynb (income + emissions joined). Only income is used in Jesus_Data.ipynb.

---

# Part E. Reproducible acquisition, cleaning and joins

## E1. Inputs and version control

| Input                        | Retrieval date / version                               | Before cleaning  | After cleaning                                                                        | Reproducible path                                                                        |
| ---------------------------- | ------------------------------------------------------ | ---------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Open Air Chicago             | 2026-10-27                                             | **121,606 x 46** | **119,859 x 15**                                                                      | [TODO Jesus: saved snapshot/API query + notebook]                                        |
| Chicago Community Population | Original export date unverified; CSV `ACS Year = 2023` | **77 x 30**      | **77 x 30** source rows retained in EDA; derived household features created in memory | `data/Chicago_Community_Population_Data.csv`; `EDA-Files/eda.ipynb`                      |
| Median Income                | 2026-09-23                                             | **78 x 5**       | **77 x 4**                                                                            | Ani_Data.ipynb (joined to Local Emission); Jesus_Data.ipynb (joined to Open Air summary) |
| Local Emission 2019          | 2026-09-28                                             | **77 x 63**      | **77 x 63**                                                                           | Ani_Data.ipynb (joined to Median Income)                                                 |

## E2. Open Air pipeline (Jesus and Claudia)

1. Retrieve the specified data snapshot through API endpoint and read into a dataframe.
2. Select the chosen columns specified above, and drop rows containing missing values in any selected column.
3. Parse the observation date and confirm **2025-07-26 to 2026-10-26**.
4. Document calibrations, duplicates, outliers and negative readings.
5. Convert sensor coordinates or verified sensor names to **actual** Chicago community areas, recording matched and unmatched sites and rows.
6. Aggregate sensor-day measurements to the geographic/time unit used in the final join, with explicit weighting and missing-day rules.

Steps in order:
```
air    = pl.read_csv("https://data.cityofchicago.org/api/v3/views/rtmx-vkjr/export.csv?accessType=DOWNLOAD&app_token=bHWsGtRFRP9x8Hl8lYivqM1hQ", infer_schema_length=10000)

air_columns = [
    "datasourceid",
    "startofperiod",
    "sensor_name",
    "no2Conc24HourMean.raw",
    "no2Conc24HourMean.value",
    "pm2_5ConcMass24HourMean.raw",
    "pm2_5ConcMass24HourMean.value",
    "pm2_5ConcMass24HourMeanUsEpaAqi.raw",
    "pm2_5ConcMass24HourMeanUsEpaAqi.value",
    "pm2_5ConcNum24HourMean.raw",
    "pm2_5ConcNum24HourMean.value",
    "latitude",
    "longitude",
    "location",
    "record_id"
]
air_select_cols = air.select(air_columns).drop_nulls()
```

## E3. Chicago Community Population pipeline (Andrew)

**Input:** `data/Chicago_Community_Population_Data.csv` (77 rows x 30 columns). Source observations were retained without missing-value row deletion. The group's `EDA-Files/eda.ipynb` contains the following transformation sequence:

```python
import polars as pl
pop = pl.read_csv('data/Chicago_Community_Population_Data.csv', infer_schema_length=10000)

income_cols = [
    'Under $25,000', '$25,000 to $49,999', '$50,000 to $74,999',
    '$75,000 to $125,000', '$125,000 +'
]
for col in income_cols:
    if pop.schema[col] == pl.String:
        pop = pop.with_columns(
            pl.col(col).str.replace_all(',', '').cast(pl.Int64)
        )

pop = pop.with_columns(pl.sum_horizontal(income_cols).alias('Total Households'))
for col in income_cols:
    pop = pop.with_columns(
        (pl.col(col) / pl.col('Total Households') * 100).alias(col + ' %')
    )

# Notebook sorts by the $125,000+ household share for a stacked bar plot.
pop = pop.sort('$125,000 + %', descending=True)
```

The first visualization is a box plot of household counts by income bracket; the second is a stacked chart of their percentage distribution across community areas. **These are derived analysis columns**; they are not additional fields in the original 30-column CSV. The code above does not filter records, so the population EDA's source-area count remains **77** (subject to any later joins not shown here). Document actual matched and unmatched row counts after integrating with air data rather than assuming 77 matches.

**Quality checks to reproduce:** `pop.shape`, `pop.null_count()`, `pop['Community Area'].n_unique()`, `pop['Record ID'].n_unique()`, and check agreement of numeric subgroup sums against `Total Population`. No source-row duplicates or null cells were found in the uploaded file.

## E4. Joins across sources (Anirudh)

The following join description is taken from the existing group draft and **must be verified against the final notebook before submission**. It concerns Open Air data and secondary income/emissions sources; it does not itself establish that the Chicago Community Population CSV was joined.

Step 1: Each sensor is mapped to a Chicago community area. The community name is extracted from the sensor_name field by removing the trailing number (e.g. "Near West Side 7" becomes "NEAR WEST SIDE") and converting to uppercase.

Step 2: The COMMUNITY field in the local emissions dataset is also converted to uppercase and trimmed, so both sides use the same format.

Step 3: A left join is performed on this standardized community name, keeping all sensor rows and attaching income and emissions data (INCOME, ONROAD_CO2) where a match exists.

Step 4: Ran the join in `Jesus_Data.ipynb`. Income data came in as 78 rows x 5 columns, one row was just header junk with no GEOID so we dropped it, leaving 77 real income records. The Open Air extract had 117,034 rows x 46 columns. After filtering out negative PM2.5 readings and grouping sensors down to one mean and one peak (90th percentile) PM2.5 value per community area, the join against the 77 income rows matched 75 areas. 2 income rows had no match.

**Geographic validation required:** Removing trailing numbers from `sensor_name` is only a reliable geographic assignment if the published sensor names are verified against community-area boundaries or an authoritative site-to-area lookup. Do not substitute a hash-based community assignment. Record unmatched names, duplicate keys, and the geographic mapping method before interpreting results.

---

# Part F. FAIR: findability, accessibility, interoperability, reusability

| FAIR dimension    | Open Air Chicago (group)                                                                                                                                                                                                                                         | Chicago Community Population (Andrew)                                                                                                                                                                                                                                                         |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Findable**      | Chicago Data Portal [landing page](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data), identifier `rtmx-vkjr`, [Data.gov catalog](https://catalog.data.gov/dataset/open-air-chicago-day-aggregations). | [Chicago Health Atlas](https://chicagohealthatlas.org/), with documented indicator pages [POP](https://chicagohealthatlas.org/indicators/POP), [PCT](https://chicagohealthatlas.org/indicators/PCT) and [INC](https://chicagohealthatlas.org/indicators/INC); retain the custom exported CSV. |
| **Accessible**    | Public CSV/API download; record the query and snapshot date.                                                                                                                                                                                                     | Public indicator pages and CSV export tool; downloaded source kept at `data/Chicago_Community_Population_Data.csv`. Exact original export timestamp not recorded.                                                                                                                             |
| **Interoperable** | Tabular machine-readable data, identifiers, timestamp and coordinates. Source-to-API column naming differences and measurement units need codebook mapping.                                                                                                      | CSV with textual community-area names, a 2023 year label, unique record IDs and comma-formatted count fields; numeric parsing and validated name matching are required.                                                                                                                       |
| **Reusable**      | Source attribution and cleaning provenance; analysis, processing and redistribution reportedly permitted under portal terms; **no named license identified**; save supporting terms link.                                                                        | Original Census/ACS attribution and Atlas citation; user confirmed access/processing/redistribution, but **no dataset-wide named license verified**. Archive selected indicators, the saved export and notebook transformations.                                                              |

Both secondary datasets are easy to find and download, no login needed, and join cleanly on community area. Reuse is weaker though, Local Emission has no listed license and is frozen at 2019, so that limit should be stated whenever it's used.

---

# Part G. People, ethics, limitations and responsible interpretation

**Open Air Chicago, preliminary scope notes:** The monitoring locations are not identifiable individual participants. The results concern places that affect residents across Chicago. Environmental-justice analysis requires attention to monitoring density, missing data, possible measurement bias, local differences within the same community area, and the difference between neighborhood-level ambient concentration and any person's exposure. A correlation with median income does not imply causation and should not stigmatize communities.

- **Open Air people/ethics expansion:** While the air quality metrics in our dataset do not reflect individuals' exposure, these data could still potentially be used to make decisions about all Chicago residents, including those living in highly industrialized or polluted neighborhoods. For example, air quality legislation relating to transportation or industrial emissions could focus on community areas with higher air pollutant levels. The same could be said for grassroots environmental protection advocacy. 
- **Community Population people/ethics (Andrew):** Data are community-level aggregates, not individual observations. Avoid attributing population averages to a person (ecological fallacy). Investigate geographic differences in air-pollution burden without treating correlation as proof of causation. Chicago community areas may contain substantial internal variation. Comparing a **2023-labeled** demographic export with **2025–2026** pollution observations entails a time mismatch; ACS sampling/nonresponse and possible source-version differences add uncertainty. Privacy concerns are lower for area-level counts than person-level data, but public results still should not stigmatize communities.
- **Cross-dataset ethics:** Mixing daily sensor data with old static snapshots can make things look more "current" than they are. All results are at the community-area level, not individual, so this describes neighborhoods, not people. Areas with fewer sensors get noisier numbers, don't read those as "worse," just less reliable. And none of this is causal, income and pollution correlating doesn't mean one causes the other.

---

# Part H. Secondary data notes (only if used in analysis)

## Median household income

Median income: Chicago Data Portal, 2020-2024 ACS estimate, one row per community area, maintained by the Census Bureau. Used to compare income against pollution. Joined by community name.

## Community-area local emissions (2019)

Local emissions: CMAP, 2019 only, maintained by CMAP. Used as a secondary pollution estimate (on-road CO2) to compare against income. Joined by community name, 2 of 77 areas didn't match.

---

# Part I. Nutrition label, references

**Nutrition label:**

[Open Air Chicago label PDF](nutrition_label/nutrition.pdf). Source: [`nutrition_label/main.tex`](nutrition_label/main.tex) and [`nutrition_label/Card.tex`](nutrition_label/Card.tex).

[Population label PDF](population/Chicago_Community_Population_Overleaf_Project.pdf). Source: [`population/main.tex`](population/main.tex) and [`population/Card.tex`](population/Card.tex).

## Sources

1. [City of Chicago Data Portal, Open Air Chicago Day Aggregations](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data), `rtmx-vkjr` (analyzed API extract retrieved 2026-10-27).
2. [Data.gov listing for Open Air Chicago Day Aggregations](https://catalog.data.gov/dataset/open-air-chicago-day-aggregations).
3. [Chicago Health Atlas home/about](https://chicagohealthatlas.org/), purpose, platform partners and ongoing updates (reviewed 2026-10-28).
4. [Chicago Health Atlas, Population (POP)](https://chicagohealthatlas.org/indicators/POP), ACS/Decennial Census notes and B01001/B03002/P012 table references (reviewed 2026-10-28).
5. [Chicago Health Atlas, Demographics (PCT)](https://chicagohealthatlas.org/indicators/PCT), subgroup-denominator and ACS notes (reviewed 2026-10-28).
6. [Chicago Health Atlas, Median household income (INC)](https://chicagohealthatlas.org/indicators/INC), contextual indicator; **not** the exact source of every household-income bracket.
7. Group file `data/Chicago_Community_Population_Data.csv` (77 rows x 30 columns; reference year 2023; original export date unverified); EDA evidence in `EDA-Files/eda.ipynb`.
8. [Chicago Data Portal, ACS 5-Year Data by Community Area](https://data.cityofchicago.org/Community-Economic-Development/ACS-5-Year-Data-by-Community-Area-Most-Recent-Year/7umk-8dtw/about_data) [TODO: if used, retrieval date].
9. [CMAP, Community Areas Local Emission 2019](https://datahub.cmap.illinois.gov/datasets/communityareas-localemission2019/about) [TODO: if used, retrieval date].
10. [City of Chicago Data Disclaimer / Terms of Use](https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html)
11. [About Open Air Chicago] (https://www.chicago.gov/city/en/depts/cdph/supp_info/Environment/open-air-chicago.html)
12. [TODO all: additional methodology/codebook references that substantiate exact income brackets and joint analysis].
