# Up or On the Rocks?
### A Time Series Analysis of Food Services and Drinking Places Employment Through the COVID-19 Recovery

## Overview
The food services and drinking places sector, which includes restaurants, bars, and similar establishments, is one of the most employment-intensive industries in the United States. 
This project examines monthly employment trends in the sector from January 2016 through 
September 2026 using data from the U.S. Bureau of Labor Statistics Current Employment 
Statistics program. The central question is straightforward: did the industry fully 
recover from the COVID-19 collapse, or is it still on the rocks?

## Data Source
- **Series:** CEU7072200001 — All Employees, Food Services & Drinking Places (NAICS 722)
- **Provider:** U.S. Bureau of Labor Statistics, Current Employment Statistics (CES)
- **Period:** January 2016 – September 2026
- **Frequency:** Monthly
- **Seasonal adjustment:** Not seasonally adjusted
- **Units:** Payroll jobs; BLS values converted from thousands to individual jobs
- **Access:** BLS Public Data API v2

A note on the data: the CES counts filled payroll positions, not unique workers. 
Employees holding multiple restaurant jobs are counted once per position, and workers 
paid off payroll are not included. The series is best understood as a measure of formal 
payroll activity in the sector rather than a headcount of individual workers.

## Methodology
**COVID-19 Treatment:** Employment changed sharply beginning in March 2020, creating an extended disruption that differed substantially from the series’ established trend and seasonal pattern. To limit this disruption’s influence on decomposition and forecasting, recorded values from March 2020 through June 2021 were replaced using linear interpolation between February 2020 and July 2021. This creates a simplified bridge across the selected period, not an estimate of actual employment or what employment would have been without the pandemic. It also removes seasonal fluctuations within that window, and the July 2021 endpoint may still reflect pandemic effects. The interpolated series is used for seasonality, autocorrelation, decomposition, and model fitting, while forecast comparisons use recorded employment. The notebook displays both series for transparency.

**Analytical Framework:** The notebook follows a structured time series workflow:
1. Full series visualization
2. COVID-period interpolation
3. Seasonality analysis including monthly averages, employment by month, overlaid by year, and STL seasonal component
4. Autocorrelation analysis
5. STL decomposition into trend, seasonal, and residual components
6. Multi-model forecasting with holdout validation
7. Comparison of the selected model’s 2026 forecast with available recorded employment

**Forecasting:** Four models were trained on the interpolated series from January 2016 through December 2024 and validated against recorded employment from January through December 2025, representing one complete seasonal cycle. Model performance was assessed using Mean Absolute Error, measured in payroll jobs.

| Model | Validation MAE (payroll jobs) |
|---|---|
| ETS | 146,720 |
| SARIMA | 45,851 |
| Seasonal Naive | 49,442 |
| OLS | 172,317 |

SARIMA produced the lowest validation MAE, approximately 7.3% below the Seasonal Naive baseline, and was selected to generate the calendar-year 2026 forecast. The (1,1,1)(1,1,1,12) specification uses first-order autoregressive and moving-average terms with regular and seasonal differencing. The selected model was refitted to the interpolated series through December 2025 and used to forecast January through December 2026, with 80% and 95% prediction intervals and a monthly forecast table. Both SARIMA fits converged. Its predictions were then compared with recorded employment from January through September 2026. These forecasts use the current data snapshot, including historical revisions, so they do not necessarily reproduce the forecasts available at the end of 2025.

## Key Findings
- Food services employment grew from approximately 10.95 million in January 2016 to a pre-pandemic peak of 12.32 million in August 2019.
- Recorded employment declined by approximately 5.7 million payroll jobs between February and April 2020, a decrease of roughly 47 percent.
- Employment first exceeded its February 2020 level in June 2022.
- Employment reached approximately 12.64 million in June 2026 before easing to 12.60 million in July and 12.58 million in August. September brought a decline of 148,400 payroll jobs from August, bringing employment to approximately 12.43 million. Employment remained above the pre-pandemic peak for six consecutive months, from April through September 2026.
- The series shows a recurring seasonal pattern, with employment generally reaching its highest levels from June through August and its lowest levels in January and February. The pandemic period should be interpreted cautiously because values from March 2020 through June 2021 were replaced through interpolation for the seasonality and modeling analyses.
- Recorded employment exceeded the SARIMA forecast in eight of the first nine months of 2026. August was the exception, coming in approximately 3,900 payroll jobs below the forecast and producing the smallest absolute forecast error so far this year. April had the largest error at approximately 72,400 jobs above forecast. September employment was approximately 27,000 jobs above forecast, and the January–September MAE was 38,149 payroll jobs.
- All recorded employment values from January through September 2026 fell within both the 80% and 95% prediction intervals. This describes coverage so far, but nine observations are not enough to establish reliable interval coverage over time.

## Tools and Libraries
- **Python** — pandas, NumPy, statsmodels, scikit-learn, Plotly, seaborn, Matplotlib, and requests
- **Environment** — Jupyter Notebook
- **Data Access** — BLS Public Data API v2

## Running the Notebook
1. Clone the repository
2. Install dependencies: `pip install pandas numpy statsmodels scikit-learn plotly seaborn matplotlib requests`
3. Register for a free BLS API key at [https://data.bls.gov/registrationEngine/](https://data.bls.gov/registrationEngine/)
4. Replace the `API_KEY` value in the first code cell with your key
5. Run all cells in order

The saved results reflect data through September 2026. Rerunning the notebook retrieves available BLS data, which may include revisions or additional months. The forecast evaluation currently selects January through September 2026 explicitly. Update that cutoff along with the reporting dates, chart labels, and written findings when refreshing the analysis.

## Planned Updates
This project is designed as a living analysis. The SARIMA model projects employment through December 2026, and newly released BLS data will be incorporated monthly. I plan to update the analysis with October 2026 employment data following the Employment Situation release scheduled for November 6, 2026. CES employment estimates are subject to revision as additional payroll information becomes available, so previously reported values and forecast errors may change.

Key questions to track during the remainder of 2026 include:

- Does recorded employment continue to exceed the SARIMA forecast in most months?
- Does employment follow the model’s overall decline toward December, including the small projected increase in October?
- How does the model’s cumulative 2026 forecast error change as additional months become available?
- Do newly recorded values continue to fall within the prediction intervals?

A companion analysis of the voluntary quit rate for the same sector provides additional context on labor-market conditions and worker retention within the industry.

## Author
Jason Staats
