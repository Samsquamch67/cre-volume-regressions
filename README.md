# CRE volume regressions

Quarterly regressions of US commercial real estate sales volume (MSCI RCA), by property type, built for the CRE services 2027 preview (CBRE, JLL, NMRK, CWK, CIGI).

## Files

| File | What it is |
|---|---|
| `cre_volume_model.csv` | **The whole model in one CSV**: all data and live formulas on one sheet. Use this to rebuild the workbook by copy-paste. |
| `RCA_Volume_Regression_by_Type.xlsx` | The same model as a formatted, multi-tab Excel workbook, with a chart. |
| `archive/RCA_Volume_Regression_total_only.xlsx` | Earlier single-equation version (total market only). |

## Rebuilding the model from the CSV

**Copy and paste (for example, inside a virtual desktop):**

1. On GitHub, open `cre_volume_model.csv`, click **Raw**, then press Ctrl+A and Ctrl+C.
2. In a new Excel workbook, click cell A1 and press Ctrl+V. Everything lands in column A.
3. With column A selected, go to Data > Text to Columns > Delimited > Next. Tick **Comma** only, set the text qualifier to `"`, then click Finish. The formulas calculate.
4. Save as `.xlsx`.

**Or download it:** click **Download raw file**, open it in Excel, then Save As `.xlsx`. You skip step 3 this way.

If the formulas show up as text, column A was formatted as Text. Set it to General and run step 3 again.

Every formula refers only to cells on the same sheet, so the links work whatever you name the sheet. Formatting and the chart don't carry over in CSV. Everything was checked: the CSV rebuild reproduces all 20,041 numbers in the Excel workbook exactly.

## Where things are in the CSV

Each block starts with a `### SHEET: name ###` row. Below it are that tab's rows from the Excel workbook, in the same columns.

| Rows | Block | What it holds |
|---|---|---|
| 1-109 | Summary | Model fit, inputs, forecasts, sensitivities |
| 112-231 | Macro | Shared drivers and derived regressors |
| 234-379 | Total | Total-market regression |
| 382-527 | Apartment | Apartment regression |
| 530-675 | Industrial | Industrial regression |
| 678-823 | Office | Office regression |
| 826-971 | Retail | Retail regression |
| 974-1119 | Hotel | Hotel regression |
| 1122-1267 | Dev_Site | Dev site regression |
| 1270-1294 | Spec_Tests | Variables tested and why kept or rejected |
| 1297-1305 | Sources | Data sources |

**Inputs to edit:** Summary rows 19-39, in columns B-J (4Q26-4Q28). These are the yellow cells in the Excel version:

- the 10-yr path, Baa spread, lending standards, 10-yr and Baa volatility, policy uncertainty, CPI, unemployment and yield curve;
- a cap rate path for each property type;
- the growth assumptions in column B.

Forecast outputs are in Summary rows 44-73.

## How the model works

- Each regression explains quarterly volume growth (log YoY). The drivers are the type's own RCA 12-month cap rate change, the 10-yr, the Baa spread, real GDP, bank lending standards, and mean reversion of volume to its share of GDP. Each model adds a few drivers of its own (see Spec_Tests).
- Coefficients come from Excel `LINEST` over 2007Q3-2026Q3 (77 quarters), so they update if you change the data.
- Forecast volume = volume four quarters earlier x EXP(model growth).

## Model fit (2007Q3-2026Q3)

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

## Data sources

- MSCI RCA Trends & Trades: sales volume by property type, property and portfolio sales, closed, excluding entity-level deals (pulled Nov 20, 2026)
- MSCI RCA Standardized Downloads: 12-month cap rates and $/sf (updated Nov 17, 2026)
- FRED: DGS10, BAA10Y, GDPC1, GDP, DRTSCLCC, ECOMPCTSA, USEPUINDXD, CPIAUCSL, UNRATE, T10Y3M
- Forecast 10-yr path: Goldman Sachs (entered by user)
