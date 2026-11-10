# Up or On the Rocks?
### A Time Series Analysis of Food Services and Drinking Places Employment Through the COVID-19 Recovery

## Overview

This project tracks monthly U.S. payroll employment in food services and drinking places from January 2016 through October 2026. It examines the recovery from the COVID-19 employment collapse, the sector’s recurring seasonal pattern, and how well ETS, SARIMA, and their 50/50 ensemble forecast employment in 2026.

- [October HTML report](up_or_on_the_rocks.html) contains the charts, forecast comparisons, and saved November and December forecasts. Download and open it in a browser for offline viewing.
- [October analysis notebook](up_or_rocks.ipynb) contains the code and saved outputs.

## Data Source
- **Series:** CEU7072200001 — All Employees, Food Services & Drinking Places (NAICS 722)
- **Provider:** U.S. Bureau of Labor Statistics, Current Employment Statistics (CES)
- **Period:** January 2016 – October 2026
- **Frequency:** Monthly
- **Seasonal adjustment:** Not seasonally adjusted
- **Units:** Payroll jobs; BLS values converted from thousands to individual jobs
- **Access:** BLS Public Data API v2

A note on the data: the CES counts filled payroll positions, not unique workers. 
Employees holding multiple restaurant jobs are counted once per position, and workers 
paid off payroll are not included. The series is best understood as a measure of formal 
payroll activity in the sector rather than a headcount of individual workers.

## Methodology
**COVID-19 Treatment.** To limit the influence of the pandemic disruption on the analysis, employment values from March 2020 through June 2021 were replaced using linear interpolation between the recorded values for February 2020 and July 2021. The resulting series bridges the disruption and removes seasonal fluctuations within that window. The July 2021 endpoint may still reflect pandemic effects. The interpolated values serve as analytical inputs. They should not be interpreted as recorded employment or as estimates of what employment would have been without the pandemic.

The notebook displays the recorded and interpolated series side by side. It uses the interpolated series for seasonality analysis, autocorrelation, STL decomposition, and fitting the ETS and SARIMA models. The 2026 evaluation scores both models and their 50/50 ensemble against recorded employment from January through October.

**Analytical Framework.** The notebook follows these steps.

1. Plot the recorded employment series from January 2016 through October 2026.
2. Interpolate employment from March 2020 through June 2021 for use in the time series analysis.
3. Examine seasonality using monthly averages, year-by-year overlays, and the estimated STL seasonal component.
4. Measure autocorrelation across monthly lags.
5. Use STL to separate the series into trend, seasonal, and residual components.
6. Evaluate four forecasting approaches on the 2025 holdout and select SARIMA based on its lowest mean absolute error (MAE).
7. Refit SARIMA through December 2025, forecast all 12 months of 2026, and evaluate its errors and prediction interval coverage against January–October actuals.
8. Explore whether averaging ETS and SARIMA reduces forecast error. Compare their monthly errors and score both models and their 50/50 ensemble against the same January–October actuals.

**2025 Forecast Validation.** Four forecasting approaches used monthly employment data through December 2024 to forecast January through December 2025. Holding out a full calendar year gave each approach the same seasonal cycle for evaluation. Forecasts were scored against recorded employment using mean absolute error (MAE), measured in payroll jobs.

| Forecasting approach | 2025 validation MAE (payroll jobs) |
|---|---:|
| SARIMA | 45,851 |
| Seasonal Naive | 49,442 |
| ETS | 146,720 |
| OLS | 172,317 |

SARIMA had the lowest 2025 validation MAE, approximately 7.3% below the Seasonal Naive baseline, so it was selected for the original 2026 forecast. The SARIMA(1,1,1)(1,1,1,12) specification uses first-order autoregressive and moving-average terms with regular and seasonal differencing. Both the validation fit and the final fit converged. The final model used the COVID-interpolated series through December 2025 to forecast all 12 months of 2026 and produce 80% and 95% prediction intervals. Its forecasts and interval coverage were evaluated against recorded employment from January through October 2026.

After reviewing SARIMA’s errors, we fitted ETS to the same series through December 2025 and averaged the ETS and SARIMA forecasts with equal weights. The follow-up comparison scored ETS, SARIMA, and the 50/50 ensemble against the same January–October actuals. Because we explored the ensemble after reviewing those errors, its MAE describes performance on the months that motivated the comparison.

The forecasts use a CES data snapshot that includes historical revisions, so they may differ from forecasts that could have been produced at the end of 2025.

## Key Findings
- Food services and drinking places employment grew from approximately 10.95 million payroll jobs in January 2016 to a pre-pandemic peak of 12.32 million in August 2019.
- Employment fell by approximately 5.7 million payroll jobs between February and April 2020, a decline of about 47%. It first exceeded its February 2020 level in June 2022.
- Employment reached approximately 12.64 million in June 2026 before easing to 12.41 million in October. Payrolls fell by 148,400 jobs in September and another 17,800 in October. Employment remained above the pre-pandemic peak for seven consecutive months, from April through October.
- Employment generally peaks from June through August and reaches its lowest levels in January and February. Both ETS and SARIMA captured the broad rise into early summer and easing into fall. The seasonality and forecasting analyses use a series with interpolated values from March 2020 through June 2021.
- SARIMA underestimated employment in eight of the ten months from January through October. August and October were the exceptions. April had the largest absolute error, with actual employment 72,421 jobs above the forecast. In October, actual employment was 23,851 jobs below the forecast.
- All ten actual values fell within both of SARIMA’s 80% and 95% prediction intervals. A longer evaluation would help establish how reliably the intervals cover future observations.
- ETS and SARIMA missed in opposite directions in six of the ten months. The 50/50 ensemble had the lowest January–October MAE at 22,513 payroll jobs, compared with 35,351 for ETS and 36,759 for SARIMA. The ensemble was explored after reviewing the errors for those months, so its lower MAE is an exploratory result.

## Tools and Libraries
- **Python** — pandas, NumPy, statsmodels, scikit-learn, Plotly, seaborn, Matplotlib, requests, and certifi
- **Environment** — Jupyter Notebook
- **Data Access** — BLS Public Data API v2

## Running the Notebook

1. Clone the repository and open a terminal in the project folder.
2. Install Jupyter Notebook and the required libraries.

   ```bash
   python -m pip install notebook pandas numpy statsmodels scikit-learn plotly seaborn matplotlib requests certifi
   ```

3. Register for a free BLS API key through the [BLS registration page](https://data.bls.gov/registrationEngine/).
4. Launch Jupyter with `python -m notebook` and open [`up_or_rocks.ipynb`](up_or_rocks.ipynb).
5. Replace the `API_KEY` placeholder in the first code cell with your key in your local copy, then run all cells from top to bottom.

The saved notebook results use employment data through October 2026. Rerunning the notebook retrieves available BLS data, which may include revisions or additional months. The forecast evaluation explicitly selects January through October 2026. When refreshing the analysis, update the evaluation cutoff, reporting dates, chart labels, and written findings together.

## Planned Updates
The ETS, SARIMA, and 50/50 ensemble forecasts extend through December 2026. The [October report](up_or_on_the_rocks.html#forecasting) records the November and December predictions. As the employment estimates become available, I’ll score those saved forecasts using the fixed 50/50 weights. The two months will provide an initial check on whether the ensemble’s lower January–October MAE carries forward.

The BLS revises Current Employment Statistics (CES) estimates as more payroll data become available, so previously reported employment levels and forecast errors may change.

Key questions to track during the remainder of 2026 include:

- Does recorded employment continue to ease toward year-end, as ETS and SARIMA project?
- Is SARIMA’s forecast below recorded employment in November or December?
- Which forecast comes closest to the November and December actuals, and how do those months change the year-to-date MAE for ETS, SARIMA, and the fixed 50/50 ensemble?
- Do the November and December actuals fall within SARIMA’s 80% and 95% prediction intervals?

A companion analysis of the sector’s voluntary quit rate provides additional context on labor-market conditions and worker retention.

## Author
Jason Staats
