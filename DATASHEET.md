# Dataset Documentation: Socioeconomic Health Disparities in Chicago

**Course:** UIC CS 418, Fall 2026  
**Project:** Socioeconomic-Health-Disparities  
**Checkpoint:** Four, Data Documentation  
**Group members:** Anirudh Gupta, Claudia Jimenez, Andrew Mcclendon, Auon Muhammad Naqvi, Jesus Rodriguez  
**Status:** **DRAFT FOR GROUP INTEGRATION**. Andrew's verified Open Air research is entered below. Other members must fill the `[TODO: ...]` fields and everyone must review before submission.

## Overview and dataset inventory

Our research examines how median household income and other community characteristics relate to air-pollution measurements across Chicago community areas. Our two **primary** datasets are Chicago Community Population and Open Air Chicago Day Aggregations. Median income and local emissions datasets may also be used to support analyses; their role and preprocessing must be documented where used.

| Dataset | Role | Source | Observation unit | Working owner(s) |
|---|---|---|---|---|
| [Open Air Chicago Day Aggregations](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data) | Primary | Chicago Data Portal | Sensor-day | Andrew; Jesus |
| [Chicago Community Population / Chicago Health Atlas](https://chicagohealthatlas.org/indicators/INC) | Primary | Chicago Health Atlas | [TODO: verify geographic and temporal observation unit] | Claudia; Auon |
| [ACS 5-Year Data by Community Area, Most Recent Year](https://data.cityofchicago.org/Community-Economic-Development/ACS-5-Year-Data-by-Community-Area-Most-Recent-Year/7umk-8dtw/about_data) | Secondary, if used | Chicago Data Portal | [TODO] | Anirudh / group |
| [Community Areas Local Emission 2019](https://datahub.cmap.illinois.gov/datasets/communityareas-localemission2019/about) | Secondary, if used | CMAP | [TODO] | Anirudh / group |

---

# Part A. Open Air Chicago Day Aggregations

**Section owners:** Andrew (publisher research, scope, FAIR and nutrition label); Jesus (codebook, reproducibility, ethical analysis, review of preprocessing and joins).  
**Source:** [Chicago Data Portal dataset `rtmx-vkjr`](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data).  
**Snapshot analyzed:** API retrieval **October 27, 2026**. Do not mix this with the earlier EDA snapshot.

## A1. Motivation

Open Air Chicago was created to provide air-quality information to Chicago neighborhoods and support community-based environmental monitoring and environmental-justice assessment. The network is jointly managed and maintained by the **Chicago Department of Public Health (CDPH)** and the **University of Illinois Chicago (UIC) School of Public Health**. Its measurements represent conditions near outdoor monitoring locations rather than the direct exposures of individual residents.

The project has received support through two grants to Dr. Serap Erdal, UIC School of Public Health:

1. **Community-Driven Air Quality and Environmental Justice Assessment**. Congressional Earmark Grant / National Institute of Standards and Technology (NIST), **Grant No. 60NANB23D206**, **October 1, 2023 to September 30, 2026**.
2. **ComEd Hyperlocal Air Quality Assessment**. **RHP Risk Management / ComEd / Exelon, LLC**, **January 16, 2025 to January 31, 2026**.

Collaborators supporting development of the network include CDOT, Clarity Movement, ComEd/Exelon, Cook County Department of Environment and Sustainability, Illinois EPA, RHP Risk Management, UIC School of Public Health, and community organizations. This partner list should not be conflated with the two organizations specifically identified as maintainers.

## A2. Composition

Each row describes a **single sensor over a complete UTC day**. The original October 27 API extract contains **121,606 rows and 46 columns**. The group's cleaned analysis extract selects 15 columns and removes any rows with missing values in those selected columns, yielding **119,859 rows and 15 columns**. Thus, **1,747 rows (about 1.44%)** were excluded and **98.56%** of records were retained.

The cleaned snapshot spans **July 26, 2025 to October 26, 2026**, with **zero unparsed observation dates**. Relevant content includes sensor identifiers, date, geographic coordinates, PM2.5 mass measurements, NO2 concentrations, and raw or cloud-calibrated measurement versions. The older EDA notebook shows that the black-carbon variables were completely missing in that earlier extract; black carbon is not one of the 15 selected analysis fields.

**Missingness/quality detail:** [TODO Jesus: Insert updated October 27 missing-value counts by relevant variable, invalid values and duplicate-identifier findings. Do not transplant old snapshot counts as if from the new snapshot.]

## A3. Collection process

The Open Air Chicago network deployed **277 sensors across all 77 Chicago community areas**. Additional sensors installed at federal monitoring sites support the development of a potential local correction factor. Therefore, 277 deployed network sensors is not necessarily the same as the count of unique sensor identifiers in the API file. Measurements come from the Clarity monitoring network and undergo automated quality-control checks and cloud calibration before daily aggregation.

Daily aggregates are reported for complete **UTC days**. The dataset's geographic positions are monitoring sites rather than sampled individuals or residences, and sensor reporting can vary over time.

**Additional methods:** [TODO Jesus: Cite the publisher's measurement methodology and document any quality flags, known site types, sampling limitations and units.]

## A4. Preprocessing, cleaning and labeling

The group's October 27 analysis selects **15 fields** from the 46-column API snapshot and performs **complete-case filtering** using `drop_nulls()` across the selected fields. This reduces the source from **121,606** to **119,859** sensor-day observations. The group plans to use **cloud-calibrated `.value` measurements** and **PM2.5 mass concentrations** for pollution analysis. Date parsing of the cleaned snapshot returned zero unparsed timestamps.

**Reproduction and important limitations:** [TODO Jesus: Add notebook/cell references; exact API query; 15 field names with types/units; field-name conversion from dotted mixed-case export to underscored lowercase API form; treatment of negative NO2 readings; duplicate tests; any additional filtering; and the exact mapping to community areas.]

## A5. Uses

The dataset supports descriptive comparisons of local pollution measurements and changes across days or locations, including community-area summaries that can be compared with socioeconomic data after a verified geographic join. It should **not** be used as a direct estimate of any resident's personal exposure or as standalone proof of regulatory compliance. Daily aggregation can hide short-duration peaks, location coverage may be uneven, and observed income/pollution associations must not be described as causal without appropriate study design.

## A6. Distribution

The dataset is publicly available from the [City of Chicago Data Portal](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data) as a **CSV download** and through a **Socrata API** such as `https://data.cityofchicago.org/resource/rtmx-vkjr.csv`. The team member verified permission for downloading, analysis, processing and redistribution. **No specific dataset license was identified.** This is a statement about the observed documentation, not a declaration that the data have no legal terms.

[TODO Anirudh: Preserve a direct link to the exact portal terms supporting reuse and include attribution requirements if stated. Record query parameters or a frozen snapshot rather than depending on a changing live endpoint.]

## A7. Maintenance and evolution

The network is jointly managed and maintained by **CDPH and UIC School of Public Health**. The City Data Portal describes **daily updates reflecting previous-day statistics**. The analyzed October 27, 2026 download had observations through October 26, 2026. As an evolving resource, dataset row counts will change with later retrievals, so the analysis should identify and preserve a consistent data snapshot.

---

# Part B. Chicago Community Population / Chicago Health Atlas

**Section owners:** Claudia and Auon.  
**Source:** [Chicago Health Atlas](https://chicagohealthatlas.org/indicators/INC).  
**Snapshot/version:** [TODO: retrieve and report exact dataset/indicator export date].

## B1. Motivation

[TODO Claudia: Purpose, original data creator/publisher, intended population and uses, funding or sourcing.]

## B2. Composition

[TODO Claudia: What a row represents; dataset dimensions for the shared snapshot; variables of interest; dates/years; reported missing values and any category suppression. Avoid treating exploratory notebook counts as the final snapshot without verification.]

## B3. Collection process

[TODO Claudia: Underlying census/survey/administrative sources, geographic aggregation, collection period, inclusion and exclusion, sampling limitations.]

## B4. Preprocessing, cleaning and labeling

[TODO Claudia/Auon: Source-side preprocessing, team's transformations, missingness, numeric parsing, keys and row counts. Link code evidence.]

## B5. Uses

[TODO Claudia: Intended and unsuitable uses, comparison limitations, demographic context and ecological inference.]

## B6. Distribution

[TODO Auon: Stable link, format, downloads or API, license, attribution and redistribution terms.]

## B7. Maintenance and evolution

[TODO Auon: Maintaining organization, update schedule, most recent publication and changes.]

---

# Part C. Data scope: population, access frame and actual samples

| Question | Open Air Chicago | Community Population (Claudia / Auon) |
|---|---|---|
| **Target population** | Chicago's 77 community areas and residents whose neighborhood outdoor-air environment is relevant to the research; residents themselves are not measured. | [TODO] |
| **Access / observation frame** | Sensors and UTC dates reporting observations through the monitoring network, including additional federal-site monitors. | [TODO] |
| **Actual analysis sample** | 119,859 complete-case **sensor-day** rows from the 2026-10-27 snapshot **before any extra geographic or income join**. | [TODO] |
| **Geographic coverage** | Network deployed in all 77 Chicago community areas, with unequal site-level coverage possible. | [TODO] |
| **Temporal coverage** | Cleaned observations 2025-07-26 to 2026-10-26; not proof of uninterrupted coverage at each site. | [TODO] |
| **Bias / exclusions** | Nonrandom sensor siting, missing days/readings, daily smoothing, calibration variability, within-area heterogeneity and ecological inference. | [TODO] |

[TODO Anirudh: Explain differences in coverage, observation grain and year between datasets and which matched community areas remain after joining.]

---

# Part D. Codebooks and variable definitions

## D1. Open Air Chicago codebook (Jesus)

[TODO: Insert complete verified **15-column codebook** from `contributions/Jesus_Open_Air_Methods.md`, including source field name, API name, definition, type, units, possible/missing values and exclusions. The API has **truncated AQI-like column names** that must be mapped to the original portal schema before making claims about their units.]

## D2. Chicago Community Population codebook (Claudia and Auon)

[TODO: Insert selected and join-relevant variables, definitions, types, units, time basis and missing-data conventions. Divide rows between Claudia and Auon as agreed.]

## D3. Secondary-dataset variables used in joins (Anirudh / group)

[TODO: Document median income and local-emissions variables actually used, their dates, units and key definitions, or state if a proposed source was not used.]

---

# Part E. Reproducible acquisition, cleaning and joins

## E1. Inputs and version control

| Input | Retrieval date / version | Before cleaning | After cleaning | Reproducible path |
|---|---|---|---|---|
| Open Air Chicago | 2026-10-27 | **121,606 x 46** | **119,859 x 15** | [TODO Jesus: saved snapshot/API query + notebook] |
| Chicago Community Population | [TODO] | [TODO] | [TODO] | [TODO Claudia/Auon] |
| Median Income | 2026-09-23 | **78 x 5** | **77 x 4** | Ani_Data.ipynb (joined to Local Emission); Jesus_Data.ipynb (joined to Open Air summary) |
| Local Emission 2019 | 2026-09-28 | **77 x 63** |**77 x 63** | Ani_Data.ipynb (joined to Median Income) |

## E2. Open Air pipeline (Jesus)

1. Retrieve the specified data snapshot through an exact download URL or stored source CSV, retaining the retrieval date: [TODO].
2. Select the 15 validated fields and report the source-to-API field mapping: [TODO].
3. Drop rows containing missing values in any selected field: **121,606 to 119,859**, **1,747 removed**.
4. Parse the observation date and confirm **2025-07-26 to 2026-10-26**, **zero unparsed**.
5. Document calibrations, duplicates, outliers and negative readings: [TODO].
6. Convert sensor coordinates or verified sensor names to **actual** Chicago community areas, recording matched and unmatched sites and rows: [TODO].
7. Aggregate sensor-day measurements to the geographic/time unit used in the final join, with explicit weighting and missing-day rules: [TODO].

## E3. Chicago Community Population pipeline (Claudia / Auon)

[TODO: Exact import, shape, missingness, transformations and normalized community-area key. Include executable notebook references.]

## E4. Joins across sources (Anirudh)

The Open Air Chicago sensor data (primary) is joined to the community-level secondary datasets (median income, local emissions) in two steps.

Step 1: Each sensor is mapped to a Chicago community area. The community name is extracted from the sensor_name field by removing the trailing number (e.g. "Near West Side 7" becomes "NEAR WEST SIDE") and converting to uppercase.

Step 2: The COMMUNITY field in the local emissions dataset is also converted to uppercase and trimmed, so both sides use the same format.

Step 3: A left join is performed on this standardized community name, keeping all sensor rows and attaching income and emissions data (INCOME, ONROAD_CO2) where a match exists.

Step 4: Match counts are printed after the join, total air rows, matched rows, unmatched rows, and matched/unmatched sensor and community area counts, to track how much data was lost in the join.

---

# Part F. FAIR: findability, accessibility, interoperability, reusability

| FAIR dimension | Open Air Chicago  | Chicago Community Population (Auon) |
|---|---|---|
| **Findable** | Chicago Data Portal [landing page](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data), identifier `rtmx-vkjr`, [Data.gov catalog](https://catalog.data.gov/dataset/open-air-chicago-day-aggregations). | [TODO] |
| **Accessible** | Public CSV/API download; record the query and snapshot date. | [TODO] |
| **Interoperable** | Tabular machine-readable data, identifiers, timestamp and coordinates. Source-to-API column naming differences and measurement units need codebook mapping. | [TODO] |
| **Reusable** | Source attribution and cleaning provenance; analysis, processing and redistribution reportedly permitted under portal terms; **no named license identified**; save supporting terms link. | [TODO] |

[TODO Anirudh: Add FAIR notes for secondary sources if central to reproducibility, and review any conflicting license claims.]

---

# Part G. People, ethics, limitations and responsible interpretation

**Open Air Chicago, preliminary scope notes:** The monitoring locations are not identifiable individual participants. The results concern places that affect residents across Chicago. Environmental-justice analysis requires attention to monitoring density, missing data, possible measurement bias, local differences within the same community area, and the difference between neighborhood-level ambient concentration and any person's exposure. A correlation with median income does not imply causation and should not stigmatize communities.

- **Open Air people/ethics expansion:** [TODO Jesus: affected communities, privacy, representation, accountability, potential harm, uncertainty, use limitations and source-backed explanation.]
- **Community Population people/ethics:** [TODO Claudia/Auon: aggregate demographics, privacy/suppression, stereotypes, sampling and equity].
- **Cross-dataset ethics:** [TODO Anirudh: geographic/temporal mismatch, ecological fallacy, reporting of low-coverage areas, careful language about disparities and prevention of causal overclaiming].

---

# Part H. Secondary data notes (only if used in analysis)

## Median household income

[TODO Anirudh / relevant analyst: Portal URL, observation year, aggregation unit, units, source/maintainer, license/terms, why it is used, transformations and join to community areas.]

## Community-area local emissions (2019)

[TODO Anirudh / relevant analyst: CMAP URL, 2019 reference year, emissions units, coverage, why it is used, and risks of comparing 2019 data with 2025-2026 sensor observations.]

---

# Part I. Nutrition label, references and group sign-off

**Nutrition label:** [Open Air Chicago label PDF](nutrition_label/nutrition.pdf). Source: [`nutrition_label/main.tex`](nutrition_label/main.tex) and [`nutrition_label/Card.tex`](nutrition_label/Card.tex).

## Sources

1. [City of Chicago Data Portal, Open Air Chicago Day Aggregations](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data), `rtmx-vkjr` (analyzed API extract retrieved 2026-10-27).
2. [Data.gov listing for Open Air Chicago Day Aggregations](https://catalog.data.gov/dataset/open-air-chicago-day-aggregations).
3. [Chicago Health Atlas, income indicator](https://chicagohealthatlas.org/indicators/INC) [TODO Claudia/Auon: exact population dataset citation and access date].
4. [Chicago Data Portal, ACS 5-Year Data by Community Area](https://data.cityofchicago.org/Community-Economic-Development/ACS-5-Year-Data-by-Community-Area-Most-Recent-Year/7umk-8dtw/about_data) [TODO: if used, retrieval date].
5. [CMAP, Community Areas Local Emission 2019](https://datahub.cmap.illinois.gov/datasets/communityareas-localemission2019/about) [TODO: if used, retrieval date].
6. [TODO Anirudh: direct City portal terms URL explaining redistribution, if not already linked in source].
7. [TODO all: methodology/codebook references that substantiate precise variable descriptions].

