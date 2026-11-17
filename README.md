# Socioeconomic-Health-Disparities

UIC, CS 418: Introduction to Data Science, Fall 2026

## Group Members

- Anirudh Gupta
- Claudia Jimenez
- Andrew Mcclendon
- Auon Muhammad Naqvi
- Jesus Rodriguez

## Research Questions

- How is median household income associated with differences in air pollution exposure across Chicago community areas?
- Which demographics are effective for predicting high pollution levels?

## Primary Datasets

- Chicago_Community_Population (source: Chicago Health Atlas, https://chicagohealthatlas.org/indicators/INC)
- Open Air Chicago Day Aggregation (source: Chicago Data Portal, https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data) NOTE: Sample in data file is small subset of whole dataset, which is downloaded from API endpoint in EDA notebook.

## Secondary Datasets

- Chicago_Median_Income_By_Community (Chicago Data Portal: https://data.cityofchicago.org/Community-Economic-Development/ACS-5-Year-Data-by-Community-Area-Most-Recent-Year/7umk-8dtw/about_data)
- CommunityAreas_LocalEmission (source: Chicago Metropolitan Agency for Planning, (https://datahub.cmap.illinois.gov/datasets/communityareas-localemission2019/about)

We plan to compare the CommunityAreas_LocalEmission dataset to the primary Open Air Chicago Day Aggregation dataset to cross-check air quality levels over time and see which neighborhoods have the most greenhouse gas emissions as a measure of pollution.

## EDA Summary

Dataset basics:

- Chicago Community Population Data: contains Chicago community areas and demographics. No null values. Analyzed number of households per community area by income bracket.
- Open Air Chicago Day Aggregations: Contains daily aggregated data since September 2025 to present from environmental sensors in various locations in and around Chicago. Investigated distributions of PM 2.5 and NO2 readings and aggregated by sensor name.
- Joined Open Air sensors with community areas and removed sensors outside of Chicago
- Community Area Local Emission dataset: Analyzed on-road CO2 emissions across Chicago community areas.
- Income dataset: Compared against mean PM 2.5 concentration from Open Air Chicago dataset.

Anomaly notes:

- The Open Air Chicago dataset contained many null values, several of which were caused by offline sensors. Irrelevant columns and rows with null values were dropped. Sensor calibration should also be investigated further.

Significant findings:

- Preliminary analysis shows most of the sensors with the highest and lowest readings are located on Chicago's South and West sides. More investigation is warranted to find any other correlations between area and air pollution.
- Preliminary analysis shows little to no linear correlation between median household income and average PM 2.5 exposure. Further investigation is likely needed.

Research questions: Our research questions may need to change in the future based on our EDA findings, especially the lack of a linear correlation between income and air quality.

Ideas:
Revised RQ1: Does household income relate more to steady, everyday PM2.5 levels or to short pollution spikes across Chicago's community areas?
Revised RQ2: Does being near industrial sites and transportation corridors predict pollution spikes better than income alone?

## Data Documentation

- **Datasheet:** [Read our complete DATASHEET.md here](DATASHEET.md)
- **Dataset 1:** [Open Air Chicago Day Aggregations](https://data.cityofchicago.org/Health-Human-Services/Open-Air-Chicago-Day-Aggregations/rtmx-vkjr/about_data) (Source: Chicago Data Portal. License: Chicago Open Data Terms of Use at https://www.chicago.gov/city/en/narr/foia/data_disclaimer.html).
- **Dataset 2:** [Chicago Community Population](https://chicagohealthatlas.org/indicators/POP) (Source: Chicago Health Atlas. License: Public Domain / Open Data).

**Terms of Use for Open Air Chicago dataset:** This site provides applications using data that has been modified for use from its original source, www.cityofchicago.org, the official website of the City of Chicago.  The City of Chicago makes no claims as to the content, accuracy, timeliness, or completeness of any of the data provided at this site.  The data provided at this site is subject to change at any time.  It is understood that the data provided at this site is being used at one’s own risk.

**Limitations:** The Open Air Chicago dataset measures outdoor neighborhood-level air quality on municipal utility poles, which does not equal an individual resident's direct personal exposure. Additionally, the daily aggregation of sensor data may hide short-duration, high-intensity localized pollution spikes, and Chicago Community Population estimates are subject to ACS sampling uncertainty.


## Main Findings (Narrative Analysis)
* Finding 1: There is no evidence of linear correlation between average PM2.5 readings by Open Air Chicago sensors and latitude or longitude. 
* Finding 2: Median household income alone did not meaningfully improve predictions of average PM2. 
* Finding 3: Areas where residents drive less have more traffic CO2 per square mile (ρ = −0.31), but controlling for density drops it to about zero (partial ρ = −0.04, CI −0.27 to 0.18). 
* Finding 4: There is sufficient evidence that the number of jobs in a community are a statistically significant predictor of on-road CO2 emissions.


### Claudia: Finding 1 
The Pearson correlation coefficient, listed as r in the table below, when plotting latitude against the average PM2.5 reading per sensor was 0.0115 with a p-value of 0.8461 and a 95% confidence interval of `[-0.1046, 0.1273]`. The Pearson correlation coefficient when plotting longitude against average PM2.5 reading was -0.0494 with a p-value of 0.4054 and a 95% confidence interval of `[-0.1644, 0.0670]`. Both r values are very close to 0, and both confidence intervals include 0, showing no statistically significant linear correlation between average PM2.5 and either latitude or longitude. This supports our EDA findings, which showed that the highest and lowest PM2.5 readings came from a mix of sensors on the north, west, and south sides of Chicago. These findings are supported by the 2D spatial scatterplot below, with each dot as one sensor with latitude plotted against longitude. There is not one singular trend of average PM2.5 increasing or decreasing as you travel north/south or east/west across the city, with most of the sensors across the city showing average PM2.5 readings near the middle of the range. The scatterplots of latitude vs average PM2.5 and longitude vs PM2.5 are also included in my [narrative analysis notebook](Narrative%20Analysis/Claudia_analysis.ipynb). I wanted to test if there was any correlation between increased PM2.5 air pollution and sensors further west or further south. These results may be confounded by Chicago’s sprawling layout and non-rectangular shape, along with the fact that major highways and transportation corridors, such as I-90/I-94 and the Chicago River, go diagonally across the city. One proposed check would be to calculate the distance between sensors with high or low PM2.5 or NO2 readings and major industrial hubs or transportation corridors. 


| x         | y             | r       | p-value | 95% CI              |
|-----------|---------------|---------|---------|---------------------|
| Latitude  | Average PM2.5 | 0.0115  | 0.8461  | `[-0.1046, 0.1273]` |
| Longitude | Average PM2.5 | -0.0494 | 0.4054  | `[-0.1644, 0.0670]` |


![2D spatial scatter](Narrative%20Analysis/Figures/Claudia_2d_spatial_scatter.jpg)

### AJ & Jesus: Finding 2

Median household income alone did not meaningfully improve the predictions of average PM2.5 concentrations across Chicago's 77 community areas. We used linear regression and compared its performance against a baseline model that always predicted the average PM2. Using five-fold cross-validation, the income-based model had an average RMSE of 0.6178 µg/m³, compared with 0.6176 µg/m³ for the baseline. This shows almost no improvement in prediction accuracy. The Pearson correlation was also weak (r = −0.1195, p = 0.3007), with a 95% confidence interval of [−0.3345, 0.1074]. One possible alternative explanation was the unusually high pollution readings. Upon further investigation, there was a wildfire that caused the sensos to spike unusally for a couple days. To test whether these extreme readings influenced our results, we excluded those two dates and repeated the analysis. Although average community PM2.5 concentrations decreased by approximately 0.634 µg/m³, the income-based model still provided only a negligible improvement over the baseline (0.5834 versus 0.5853 µg/m³ RMSE). We also explored the number of jobs in a community and it is a really strong predictor for on-road CO2 emissions ($p < 0.001$, $R^2 = 0.704$ in a log-log model). These findings suggest that median household income alone is not a reliable linear predictor of average PM2.5 concentrations in our dataset.
![My Graph](Narrative%20Analysis/Figures/AJ-IncomeRegression.png)

### Ani: Finding 3

I compared how much residents drive with traffic CO2 per square mile, dividing by area so larger community areas aren't penalized just for their size. Areas where residents drive less have more traffic CO2 per square mile (ρ = -0.31, CI -0.53 to -0.07), and the permutation test (p = 0.006) shows this is unlikely to be chance. The 20 areas with low resident driving but high traffic emissions have about $21,000 higher median income than the rest (CI $6,478 to $59,347, Mann-Whitney p = 0.0015). Controlling for density, the correlation drops to about zero (partial ρ = -0.04, CI -0.27 to 0.18), so density explains most of it. Without the five biggest job centers, the correlation weakens to ρ = -0.24 (CI -0.47 to 0.01) and its CI includes zero. The driving vs. emissions link is mostly a density effect, but the finding that mismatch areas are dense, central and higher income holds up, and none of this shows causation. ONROAD_CO2 is modeled, not measured (it could be checked against Open Air NO2), and with only 77 areas the confidence intervals are wide. All numbers come from my 
![Resident driving vs. traffic emissions](Narrative%20Analysis/Figures/ani_driving_vs_traffic.png)

### Auon: Finding 4

# Analysis

Based on the below correlation coefficienct matrix, taken from the eda notebook, we can see that there's a pearson r coefficient of 0.800 between number of jobs and Onroad CO2 emissions. I decided to explore this connection further by trying to create a Linear Regression Model to see if we can justify Income having any explanatory power on Onroad CO2. 

| Variable | POP | JOBS | INCOME | ONROAD_CO2 | NONRES_ELE |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **POP** | 1.0 | 0.259841 | 0.310621 | 0.456785 | 0.404039 |
| **JOBS** | 0.259841 | 1.0 | 0.370146 | 0.800138 | 0.91646 |
| **INCOME** | 0.310621 | 0.370146 | 1.0 | 0.357041 | 0.385903 |
| **ONROAD_CO2** | 0.456785 | 0.800138 | 0.357041 | 1.0 | 0.83165 |
| **NONRES_ELE** | 0.404039 | 0.91646 | 0.385903 | 0.83165 | 1.0 |

However upon performing the LINE test for the linear regression model, it was quite clear to see that multiple assumptions were not met. the Q-Q plot is not linear, Equal variance of errors is not met (distribution heavily skewed right). Using Boxcox (procedure not shown), I determined that a log regression model was most appropriate. After performing this transformation I now did a LINE test for the log regression model, and the results were much better. Normality was now met, and Linearity and Equal Variance were reasonably met. The real problem is independence, as there does seem to be a very slight upwards pattern in the residuals, *however* this could also be because of the order of the communities in the dataset. To further verify I found the Breusch-Pagan p-value which was 0.045, just slightly under 0.05, so we cannot entirely rule out the possibility of dependence. To address this limitation, we used HC3 heteroskedasticity-robust standard errors, where we got p < 0.001, with a 95% CI of (0.475, 0.640), B_1 = 0.5572. As our 95% CI does not include 0 and we have a P-value of less than 0.05, we determine that the relationship between jobs and on-road CO2 emissions remains statistically significant. However this does not mean jobs impacts these emissions.

To verify whether or not Jobs significantly impact variability in Onroad CO2 emissions, we perform a two sided Wald Z test. From the results in my Narrative Analysis notebook, we can see that we reject our Null hypothesis, meaning there is sufficient evidence that the number of jobs in a community are a statistically significant predictor of on-road CO2 emissions.

![Job count vs Onroad CO2 Emissions](Narrative%20Analysis/Figures/Auon-JobsVsOnroadCO2LogRegression.png)
