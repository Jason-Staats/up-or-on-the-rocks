# Up or On the Rocks?
### A Time Series Analysis of Restaurant Job Recovery Post-COVID-19

## Overview
The food services and drinking places sector, which includes restaurants, bars, and similar establishments, is one of the most employment-intensive industries in the United States. 
This project examines monthly employment trends in the sector from January 2016 through 
August 2026 using data from the U.S. Bureau of Labor Statistics Current Employment 
Statistics program. The central question is straightforward: did the industry fully 
recover from the COVID-19 collapse, or is it still on the rocks?

## Data Source
- **Series:** CEU7072200001 — All Employees, Food Services & Drinking Places (NAICS 722)
- **Provider:** U.S. Bureau of Labor Statistics, Current Employment Statistics (CES)
- **Period:** January 2016 – August 2026
- **Frequency:** Monthly
- **Units:** Payroll jobs; BLS values converted from thousands to individual jobs
- **Access:** BLS Public Data API v2

A note on the data: the CES counts filled payroll positions, not unique workers. 
Employees holding multiple restaurant jobs are counted once per position, and workers 
paid off payroll are not included. The series is best understood as a measure of formal 
payroll activity in the sector rather than a headcount of individual workers.

## Methodology
**COVID-19 Treatment:** Employment changed sharply beginning in March 2020, creating an extended disruption that differed substantially from the series’ established trend and seasonal pattern. To limit this disruption’s influence on decomposition and forecasting, recorded values from March 2020 through June 2021 were replaced with a linear interpolation that creates a hypothetical smooth path between the observations before and after the selected period. The interpolated series is used for subsequent seasonality, autocorrelation, decomposition, and forecasting analyses. These interpolated values should not be interpreted as estimates of actual employment. The notebook displays both the recorded and interpolated series for transparency.

**Analytical Framework:** The notebook follows a structured time series workflow:
1. Full series visualization
2. COVID-period interpolation
3. Seasonality analysis including monthly averages, year-over-year overlay, and STL seasonal component
4. Autocorrelation analysis
5. STL decomposition into trend, seasonal, and residual components
6. Multi-model forecasting with holdout validation
7. Comparison of the selected model’s 2026 forecast with available recorded employment

**Forecasting:** Four models were trained on the interpolated series from January 2016 through December 2024 and validated against recorded employment from January through December 2025, representing one complete seasonal cycle. Model performance was assessed using Mean Absolute Error, measured in payroll jobs.

| Model | Validation MAE |
|---|---|
| ETS | 146,720 |
| SARIMA | 45,851 |
| Seasonal Naive | 49,442 |
| OLS | 172,317 |

SARIMA produced the lowest validation MAE and was selected to generate the calendar-year 2026 forecast. The (1,1,1)(1,1,1,12) specification was retained after nearby model orders produced no meaningful improvement in validation performance. The selected model was refitted to the interpolated series through December 2025 and used to forecast January through December 2026, with 80% and 95% prediction intervals and a monthly forecast table. Its predictions were then compared with recorded employment from January through August 2026.

## Key Findings
- Food services employment grew from approximately 10.95 million in January 2016 to a pre-pandemic peak of 12.32 million in August 2019.
- Recorded employment declined by approximately 5.7 million payroll jobs between February and April 2020, a decrease of nearly 50 percent.
- Employment first exceeded its February 2020 level in June 2022.
- Employment reached approximately 12.64 million in June 2026 before easing slightly in July and August, both of which round to 12.62 million. Employment remained above the pre-pandemic peak for five consecutive months, from April through August 2026.
- The series shows a recurring seasonal pattern, with employment generally reaching its highest levels from June through August and its lowest levels in January and February. The pandemic period should be interpreted cautiously because values from March 2020 through June 2021 were replaced through interpolation for the seasonality and modeling analyses.
- Recorded employment exceeded the SARIMA forecast in every month from January through August 2026. Monthly differences ranged from approximately 29,000 payroll jobs in August to 72,000 in April. The model’s Mean Absolute Error over the eight-month period was approximately 44,800 jobs, with August producing the smallest forecast error so far this year.
- All recorded employment values from January through August 2026 fell within both the 80% and 95% prediction intervals.

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

The saved results reflect data through August 2026. Rerunning the notebook retrieves available BLS data, which may include revisions or additional months. Update the reporting dates, chart labels, and written findings when refreshing the analysis.

## Planned Updates
This project is designed as a living analysis. The SARIMA model projects employment through December 2026, and newly released BLS data will be incorporated monthly. I plan to update the analysis with September 2026 employment data following the Employment Situation release scheduled for October 2, 2026. CES employment estimates are subject to revision as additional payroll information becomes available, so previously reported values and forecast errors may change.

Key questions to track during the remainder of 2026 include:

- Does recorded employment remain slightly above the SARIMA forecast?
- Does employment follow the model’s overall decline toward December, including the small projected increase in October?
- How does the model’s cumulative 2026 forecast error change as additional months become available?
- Do newly recorded values continue to fall within the prediction intervals?

A companion analysis of the voluntary quit rate for the same sector provides additional context on labor-market conditions and worker retention within the industry.

## Author
Jason Staats
