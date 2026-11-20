# CRE volume regressions

Quarterly regressions of US commercial real estate sales volume (MSCI RCA), by property type, built for the CRE services 2027 preview (CBRE, JLL, NMRK, CWK, CIGI).

**Private repository.** The workbooks contain MSCI RCA data. MSCI's terms restrict redistribution and use of its data with AI tools unless licensed, so keep this repository private and check your firm's license before sharing.

## Files

| File | What it is |
|---|---|
| `RCA_Volume_Regression_by_Type.xlsx` | Main workbook: one regression per property type plus total, quarterly forecast 4Q26-4Q28 |
| `archive/RCA_Volume_Regression_total_only.xlsx` | Earlier single-equation version (total market only), kept for reference |

## How the main workbook works

The workbook is self-contained. Every formula links to cells inside the same file, with no links to other files, so the links keep working wherever you open it.

- **Summary**: the only tab you edit. Change the yellow cells: the 10-yr path, Baa spread, lending standards, rate volatility, policy uncertainty, CPI, unemployment, yield curve, GDP growth, e-commerce share, industrial $/sf growth, and a cap rate path for each property type, all by quarter. The tab also shows model fit, quarterly and annual forecasts by type, sensitivities and a chart.
- **Macro**: shared drivers (FRED data plus your forecast inputs), and the regressors derived from them.
- **Total, Apartment, Industrial, Office, Retail, Hotel, Dev_Site**: one tab per regression. The top shows the coefficients, which come from Excel `LINEST` over 2007Q3-2026Q3 and update if you change the data. Below that are the quarterly data, the regressors, the fitted values and the forecast.
- **Spec_Tests**: the variables tested and why they were kept or rejected.
- **Sources**: data sources and definitions.

Color code: blue = reported data, black = formulas, green = links, yellow = your inputs, grey rows = forecast quarters.

## Model fit (2007Q3-2026Q3, 77 quarters)

| Model | R-squared | Out-of-sample MAE (log pts) |
|---|---|---|
| Total | 0.947 | 0.148 |
| Apartment | 0.903 | 0.194 |
| Industrial | 0.863 | 0.154 |
| Office | 0.908 | 0.201 |
| Retail | 0.848 | 0.186 |
| Hotel | 0.884 | 0.409 |
| Dev site | 0.760 | 0.212 |

Out-of-sample: estimated through 2021Q4, predicting 2022Q1-2026Q3.

## Updating the data

To roll forward a quarter, add the new RCA volume and 12-month cap rates (RCA Standardized Downloads, US All - Volume and Pricing) and the FRED series listed on the Sources tab in the next row of each tab. Then extend the `LINEST` ranges at the top of each regression tab.

## Data sources

- MSCI RCA Trends & Trades: sales volume by property type, property and portfolio sales, closed, excluding entity-level deals (pulled Nov 20, 2026)
- MSCI RCA Standardized Downloads: 12-month cap rates and $/sf (updated Nov 17, 2026)
- FRED: DGS10, BAA10Y, GDPC1, GDP, DRTSCLCC, ECOMPCTSA, USEPUINDXD, CPIAUCSL, UNRATE, T10Y3M
- Forecast 10-yr path: Goldman Sachs (entered by user)
