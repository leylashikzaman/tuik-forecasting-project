# TÜİK Tourism Forecasting Project

## 1. Project Overview

This project develops a quarterly forecasting model for domestic tourism in Turkey using official TÜİK data. The selected variable is the total number of trips made by domestic visitors, observed quarterly from 2009 Q1 to 2025 Q4. Ten alternative forecasting methods are applied, compared using standard accuracy measures, and the superior method is selected to forecast 2026 Q1.

## 2. Data Source and TÜİK Connection

Data were accessed directly from the TÜİK Data Portal using the `tuikr` R package combined with `httr::GET()` to download the istab file. No data was manually downloaded, edited, or saved outside of R.

- **TÜİK dataset name:** Number of Trips of Domestic Visitors by Type of Accommodation and Quarter
- **TÜİK theme/category:** Tourism (Theme ID: 14)
- **TÜİK table name:** Number of Trips of Domestic Visitors by Type of Accommodation and Quarter
- **tuikr dataflow ID:** NA (istab type, no SDMX dataflow ID; accessed via statistical_tables() + httr::GET())
- **Selected variable:** Total number of domestic trips (thousands)
- **Data frequency:** Quarterly
- **Time coverage:** 2009 Q1 – 2025 Q4
- **Latest available observation:** 2025 Q4
- **Forecast target period:** 2026 Q1
- **Date of data access:** 2026-06-02
- **R package used for data access:** `tuikr`
- **Package source:** https://github.com/emraher/tuikr

## 3. Research Objective

This project forecasts the total number of domestic trips in Turkey for 2026 Q1. Domestic tourism is an economically significant indicator reflecting household travel behavior, seasonal mobility patterns, and the overall health of the Turkish tourism sector. Quarterly frequency makes it well suited for seasonal forecasting methods.

## 4. Use of TÜİK Data in R

TÜİK data were imported entirely within R using the `tuikr` package. The `statistical_tables("14")` function was used to retrieve the Tourism theme table catalog. The target table URL was extracted from row 57 and downloaded using `httr::GET()` with session handling. The raw Excel file was read using `readxl::read_excel()`. All cleaning steps — year filling, quarter filtering, numeric conversion, and time series structuring — were performed within the R notebook using reproducible code. No manually prepared or externally saved data file was used at any stage.

## 5. Exploratory Time Series Analysis

The series shows a clear seasonal pattern with Q3 consistently being the highest quarter (summer peak) and Q1 the lowest. A mild upward trend is visible from 2009 to 2019, followed by a sharp structural break in 2020 due to the COVID-19 pandemic. Recovery is visible from 2021 onward, with the series approaching pre-pandemic levels by 2024–2025. No missing values were found in the quarterly series.

## 6. Forecasting Methods Applied

- **Naïve Forecasting** — applicable, used as baseline
- **Moving Average (window=4)** — applicable, one-year window appropriate for quarterly data
- **Weighted Moving Average** — applicable, weights 0.1, 0.2, 0.3, 0.4 giving more importance to recent quarters
- **Exponential Smoothing** — applicable, alpha=0.3
- **Trend-Adjusted Exponential Smoothing** — applicable, series shows trend; alpha=0.3, beta=0.1
- **Linear Trend Projection** — applicable, captures long-run trend
- **Seasonal Indices** — applicable, strong quarterly seasonality present
- **Additive Decomposition** — applicable, seasonal variation appears roughly constant in magnitude
- **Multiplicative Decomposition** — applicable, tested as alternative to additive
- **Regression with Trend and Seasonal Dummy Variables** — applicable, combines trend and seasonality explicitly

## 7. Forecast Accuracy Comparison

See `outputs/tables/accuracy_comparison.csv` for the full table with Bias, MAD, MSE, MAPE, RSFE, and Tracking Signal for all ten methods.

## 8. Selection of the Superior Method

Regression with Trend and Seasonal Dummy Variables was selected as the superior method. It explicitly models both the long-run trend and the quarterly seasonal pattern using a time index and three seasonal dummy variables. Among all methods, it produces one of the lowest MAPE values and its tracking signal indicates no systematic bias. Crucially, it is the most structurally appropriate method for this series because the data clearly exhibits both trend and seasonality — which regression with dummies is specifically designed to capture simultaneously.

## 9. Final Next-Period Forecast

- **Superior method:** Regression with Trend and Seasonal Dummy Variables
- **Date of data access:** 2026-06-02
- **Latest available TÜİK observation:** 2025 Q4
- **Forecast target period:** 2026 Q1
- **Forecasted value:** see `outputs/tables/final_forecast.csv`

## 10. Interpretation of Results

The forecast for 2026 Q1 reflects the historically low first-quarter demand for domestic tourism in Turkey. Q1 consistently records the fewest trips due to winter conditions and the post-holiday slowdown. The regression model captures this seasonal pattern alongside the gradual recovery trend observed since 2021. The forecasted value is consistent with recent Q1 observations and the upward trajectory of the series.

## 11. Limitations

- The 2020 COVID-19 pandemic caused a major structural break that may distort trend estimates
- A linear trend assumption may not hold beyond the sample period
- The model does not incorporate external variables such as fuel prices, exchange rates, or policy changes
- Quarterly frequency limits the granularity of seasonal analysis
- Data revisions by TÜİK could affect reproducibility if re-run at a later date

## 12. Reproducibility

1. Clone the repository: `git clone https://github.com/YOUR_USERNAME/tuik-forecasting-project.git`
2. Open `forecasting_project.Rmd` in RStudio
3. Install dependencies: `renv::restore()`
4. Click **Knit** to render the full analysis
5. All outputs will be generated automatically in `outputs/tables/` and `outputs/figures/`

No manual data preparation is required. Data is fetched live from TÜİK at knit time.

## 13. Repository Structure

```
tuik-forecasting-project/
├── README.md
├── forecasting_project.Rmd
├── forecasting_project.html
├── outputs/
│   ├── tables/
│   │   ├── accuracy_comparison.csv
│   │   └── final_forecast.csv
│   └── figures/
│       ├── actual_series_plot.png
│       ├── naive_forecast_plot.png
│       ├── moving_average_plot.png
│       ├── weighted_moving_average_plot.png
│       ├── exponential_smoothing_plot.png
│       ├── trend_adjusted_smoothing_plot.png
│       ├── trend_projection_plot.png
│       ├── seasonal_indices_plot.png
│       ├── additive_decomposition_plot.png
│       ├── multiplicative_decomposition_plot.png
│       ├── regression_seasonal_dummy_plot.png
│       └── superior_method_plot.png
├── R/
│   ├── data_import.R
│   ├── forecasting_methods.R
│   ├── accuracy_measures.R
│   └── plots.R
├── renv.lock
└── .gitignore
```
