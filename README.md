# Up or On the Rocks?
### A Time Series Analysis of Restaurant Job Recovery Post-COVID-19

## Overview
The food services and drinking places sector, which includes restaurants, bars, and similar establishments, is one of the most employment-intensive industries in the United States. 
This project examines monthly employment trends in the sector from January 2016 through 
July 2026 using data from the U.S. Bureau of Labor Statistics Current Employment 
Statistics program. The central question is straightforward: did the industry fully 
recover from the COVID-19 collapse, or is it still on the rocks?

## Data Source
- **Series:** CEU7072200001 — All Employees, Food Services & Drinking Places (NAICS 722)
- **Provider:** U.S. Bureau of Labor Statistics, Current Employment Statistics (CES)
- **Period:** January 2016 – July 2026
- **Frequency:** Monthly
- **Units:** Payroll jobs; BLS values converted from thousands to individual jobs
- **Access:** BLS Public Data API v2

A note on the data: the CES counts filled payroll positions, not unique workers. 
Employees holding multiple restaurant jobs are counted once per position, and workers 
paid off payroll are not included. The series is best understood as a measure of formal 
payroll activity in the sector rather than a headcount of individual workers.

## Methodology
**COVID-19 Treatment:** Employment changed sharply beginning in March 2020, creating an extended disruption that differed substantially from the series’ established trend and seasonal pattern. To limit this disruption’s influence on decomposition and forecasting, recorded values from March 2020 through June 2021 were replaced with a linear interpolation that creates a hypothetical smooth path between the observations before and after the selected period. These interpolated values are used for modeling only and should not be interpreted as estimates of actual employment. The notebook displays both the recorded and interpolated series for transparency.

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

SARIMA produced the lowest validation MAE and was selected to generate the calendar-year 2026 forecast. Its predictions were then compared with available recorded employment from January through July 2026.

## Key Findings
- Food services employment grew from approximately 10.95 million in January 2016 to a pre-pandemic peak of 12.32 million in August 2019.
- Recorded employment declined by approximately 5.7 million payroll jobs between February and April 2020, a decrease of nearly 50 percent.
- Employment first exceeded its February 2020 level in June 2022.
- Employment reached 12.66 million in June 2026 and remained relatively steady at 12.62 million in July. Both months were above the previous peak recorded in August 2019.
- The series shows a recurring seasonal pattern, with employment generally reaching its highest levels from June through August and its lowest levels in January and February. The pandemic period should be interpreted cautiously because values from March 2020 through June 2021 were replaced through interpolation for the seasonality and modeling analyses.
- Recorded employment remained slightly above the SARIMA forecast in every month from January through July 2026. Monthly differences ranged from approximately 39,000 to 72,000 payroll jobs, and the model’s Mean Absolute Error over the seven-month period was approximately 49,000 jobs. June employment was 12.66 million compared with a forecast of 12.61 million, while July employment was 12.62 million compared with a forecast of 12.57 million.

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

## Planned Updates
This project is designed as a living analysis. The SARIMA model projects employment through December 2026, and newly released BLS data will be incorporated monthly. CES employment estimates are subject to revision as additional payroll information becomes available. The next scheduled Employment Situation release, covering August 2026, is September 4, 2026.

Key questions to track during the remainder of 2026 include:

- Does recorded employment remain slightly above the SARIMA forecast?
- Does employment follow the model’s projected decline after the summer peak?
- How does the model’s cumulative 2026 forecast error change as additional months become available?


A companion analysis of the voluntary quit rate for the same sector provides additional context on labor-market conditions and worker retention within the industry.

## Author
Jason Staats
