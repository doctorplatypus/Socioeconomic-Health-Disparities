## Main Findings (Narrative Analysis)

- Median household income alone did not meaningfully improve predictions of average PM2.
- Median household income also showed little predictive value for average NO₂ concentrations across Chicago communities.

### Finding One: Median Household Income and PM2.5

Median household income alone did not meaningfully improve the predictions of average PM2.5 concentrations across Chicago's 77 community areas. To investigate this, we used linear regression and compared its performance against a baseline model that always predicted the average PM2.5 concentration. Using five-fold cross-validation, the income-based model had an average RMSE of 0.6178 µg/m³, compared with 0.6176 µg/m³ for the baseline. This shows almost no improvement in prediction accuracy. The Pearson correlation was also weak (r = −0.1195, p = 0.3007), with a 95% confidence interval of [−0.3345, 0.1074]. One possible alternative explanation was the unusually high pollution readings. Upon further investigation, there was a wildfire that caused the sensos to spike unusally for a couple days. To test whether these extreme readings influenced our results, we excluded those two dates and repeated the analysis. Although average community PM2.5 concentrations decreased by approximately 0.634 µg/m³, the income-based model still provided only a negligible improvement over the baseline (0.5834 versus 0.5853 µg/m³ RMSE). These findings suggest that median household income alone is not a reliable linear predictor of average PM2.5 concentrations in our dataset.
