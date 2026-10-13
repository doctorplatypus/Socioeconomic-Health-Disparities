# Exploratory Data Analysis

Summary of work done in [`eda.ipynb`](eda.ipynb).

## 1. Dataset Profiles

| Dataset | Shape | Key observations |
|---|---|---|
| Chicago Community Population | 77 × 30 | Population, age, race, and income brackets; no nulls reported. |
| Open Air Chicago | 117,320 × 46 | Daily sensor readings (PM2.5, NO2, weather); several columns have many nulls. |
| Community Area Local Emissions (2019) | 77 × 67 | Jobs, income, energy use, and emissions; 77 distinct communities and no duplicate rows. |
| Chicago Median Income | 78 × 5 initially | One header-like data row made income fields strings; removed during cleaning. |

The cleaned income–air-quality join has **75 rows × 6 columns**. Column types, null counts, and summary statistics are displayed in the notebook.

## 2. Anomalies and Cleaning

| Finding | Rows affected | Action |
|---|---:|---|
| Open Air nulls in selected columns | 1,665 | Dropped, leaving 115,655 rows across 15 selected columns. |
| Negative NO2 values (minimum −28.86) | Not counted | Kept and flagged for investigation. |
| Emissions: 10 constant columns | 77 | Flagged as unhelpful for comparisons. |
| Emissions duplicates | 0 | No action needed. |
| Income: non-data first row | 1 | Removed and converted income fields to numeric. |
| Unmatched sensor names in a 1,000-row test join | 15 | Removed from that merged sample. |

## 3. Distributions

| Feature | Center and spread | Shape / outliers |
|---|---|---|
| Household income brackets (population data) | Median ~800–1,600 households per bracket | Right-skewed; $125k+ bracket has the greatest variability. |
| PM2.5 | Median 8.87; mean 10.81; SD 11.35 µg/m³ | Right-skewed; high readings around 50–250. |
| NO2 | Median 14.45; mean 15.22; SD 4.47 ppb | Slight right skew; high readings around 25–55. |
| Jobs | Center ~10,000; range ~500–120,000 | Strong right skew; 8 outlier communities. |
| On-road CO2 | Median 74,098; mean 111,442; range 8,842–679,090; IQR 66,979 | Strong right skew; 5 high outliers, including Loop and O'Hare. |
| Median household income (joined data) | Median ~$71,400 | Right-skewed; upper outliers >$150,000. |
| Mean PM2.5 (joined data) | Center ~17.5–18.2 µg/m³ | Unimodal. |

## 4. Relationships and 5. So What?

- **Highest/lowest sensor-average PM2.5:** Among 286 sensor names, the notebook compares the top and bottom 10. Higher readings often appear on the West and South sides, but low readings at some South Side sensors and O'Hare complicate that pattern. **Next:** check sensor surroundings and geography.
- **Emissions correlation matrix:** Jobs correlate strongly with nonresidential electricity (**r = 0.916**) and on-road CO2 (**r = 0.800**); on-road CO2 and nonresidential electricity also correlate (**r = 0.832**). Activity-related features may matter more than income for emissions. **Next:** examine these relationships by community type.
- **Income vs. mean PM2.5:** Nearly flat relationship (**r = −0.09**). Income alone does not explain much of the variation. **Next:** compare against transportation proximity.
- **Peak PM2.5 by income quartile:** Lower-income groups average peaks around **35.0 µg/m³**, versus about **33.8 µg/m³** in the mid-high group. Differences are small. **Next:** investigate local pollution sources.
- **Income/air-quality correlation matrix:** Income correlates weakly with peak PM2.5 (**r = −0.15**), while mean and peak PM2.5 correlate strongly (**r = 0.91**). **Next:** test location and traffic factors instead of relying on income alone.

Tl;dr this is all evidence of potential correlation, not definitive evidence of causation.

## 6. What Next? Revised Questions

1. Does household income relate more to average PM2.5 levels or short pollution spikes across Chicago communities?
2. Does proximity to industrial sites and transportation corridors predict pollution spikes better than income alone?

The weak income–PM2.5 relationships shifted our focus toward location, industry, and major roadways (including I-55 and I-90/94). This is going to be the work we do next, ideally anyway.
