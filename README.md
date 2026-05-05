# Up or On the Rocks?
### A Time Series Analysis of Restaurant Job Recovery Post-Covid-19

## Overview
The food services and drinking places sector — restaurants, bars, and similar 
establishments — is one of the most employment-intensive industries in the United States. 
This project examines monthly employment trends in the sector from January 2016 through 
March 2026 using data from the U.S. Bureau of Labor Statistics Current Employment 
Statistics program. The central question is straightforward: did the industry fully 
recover from the COVID-19 collapse, or is it still on the rocks?

## Data Source
- **Series:** CEU7072200001 — All Employees, Food Services & Drinking Places (NAICS 722)
- **Provider:** U.S. Bureau of Labor Statistics, Current Employment Statistics (CES)
- **Period:** January 2016 – March 2026
- **Frequency:** Monthly
- **Units:** Employees (raw count)
- **Access:** BLS Public Data API v2

A note on the data: the CES counts filled payroll positions, not unique workers. 
Employees holding multiple restaurant jobs are counted once per position, and workers 
paid off payroll are not included. The series is best understood as a measure of formal 
payroll activity in the sector rather than a headcount of individual workers.

## Methodology
**COVID-19 Treatment:** The pandemic caused an unprecedented collapse in employment 
beginning in March 2020, representing an external shock rather than a feature of the 
underlying trend or seasonal pattern. Employment values from March 2020 through June 2021 
were replaced with linearly interpolated estimates to preserve the integrity of the trend 
and seasonal components. The original and interpolated series are both shown in the 
notebook for full transparency.

**Analytical Framework:** The notebook follows a structured time series workflow:
1. Full series visualization
2. Seasonality analysis including monthly averages, year-over-year overlay, and STL seasonal component
3. Autocorrelation analysis
4. STL decomposition into trend, seasonal, and residual components
5. Multi-model forecasting with holdout validation

**Forecasting:** Four models were trained on January 2016 through December 2024 and 
validated against actual January through December 2025 employment, representing one 
complete seasonal cycle. Model performance was assessed using Mean Absolute Error.

| Model | Validation MAE |
|---|---|
| ETS | 51,719 |
| SARIMA | 45,851 |
| Seasonal Naive | 49,442 |
| OLS | 172,317 |

SARIMA produced the lowest error and was selected to generate the 2026 forward forecast.

## Key Findings
- Food services employment grew steadily from approximately 11 million in early 2016 to 
a pre-pandemic peak of 12.3 million by late 2019
- The COVID-19 pandemic caused a loss of more than 5 million jobs in a matter of weeks 
in spring 2020
- Recovery was swift relative to other sectors, with employment returning to pre-pandemic 
levels by mid-2023
- The series has plateaued near 12.2 million since 2023, suggesting the post-pandemic 
rebound may have run its course
- A consistent seasonal pattern persists across all years, with employment peaking in 
June through August and reaching its lowest point in January and February
- Actual January through March 2026 employment came in below the SARIMA forecast, likely 
reflecting the impact of tariff increases and broader macroeconomic uncertainty

## Tools and Libraries
- **Python** — pandas, numpy, statsmodels, scikit-learn, plotly, seaborn
- **Environment** — Jupyter Notebook
- **Data Access** — BLS Public Data API v2

## Running the Notebook
1. Clone the repository
2. Install dependencies: `pip install pandas numpy statsmodels scikit-learn plotly seaborn requests`
3. Register for a free BLS API key at [https://data.bls.gov/registrationEngine/](https://data.bls.gov/registrationEngine/)
4. Replace the `API_KEY` value in the first code cell with your key
5. Run all cells in order

## Planned Updates
This project is designed as a living analysis. The SARIMA model projects employment 
through December 2026, and actual BLS data will be incorporated monthly as releases 
become available. The next scheduled BLS Employment Situation release covering April 2026 
data is May 8, 2026. Key questions to track throughout 2026:

- Does the forecast gap widen or narrow as the year progresses?
- Does the summer seasonal peak materialize at the expected magnitude?
- Do tariff and macroeconomic headwinds produce a measurable sustained impact on 
employment levels?

A companion analysis examining the voluntary quit rate for the same sector is also in 
development, which will provide additional context on labor market health and worker 
retention trends in the industry.

## Author
Jason Staats  
[LinkedIn](https://www.linkedin.com/in/your-profile) | [GitHub](https://github.com/your-handle)
