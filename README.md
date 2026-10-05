# Victoria Electricity Demand Forecasting

![R](https://img.shields.io/badge/R-4.5-276DC3?logo=r&logoColor=white) ![MSc](https://img.shields.io/badge/MSc-Coursework-8A2BE2)

Forecasting daily electricity demand in Victoria, Australia, 98 days ahead with a dynamic harmonic regression: ARIMA errors, Fourier terms for the weekly cycle, and temperature and calendar predictors. MSc coursework for *Time Series Analysis for Business* at Queen Mary University of London.

## Result

The chosen model, regression with ARIMA(4,1,1) errors plus Fourier terms, beat the seasonal naive baseline by about 22% on RMSE and 26% on MAE over the 98-day test period (1 July to 6 October 2020).

| Model | RMSE (MWh) | MAE (MWh) | MAPE |
|---|---|---|---|
| Seasonal naive | 18,134 | 14,603 | 13.23% |
| Dynamic harmonic regression | 14,117 | 10,836 | 9.87% |

It also had the lowest AICc and BIC of the fitted models:

| Model | AICc | BIC |
|---|---|---|
| Holt-Winters | 51,356.0 | 51,423.1 |
| Regression with ARIMA errors | 40,280.2 | 40,336.1 |
| Dynamic harmonic regression | 39,787.8 | 39,877.2 |

## Approach

1. **Data:** 2,106 days (January 2015 to October 2020) of demand alongside temperature, solar exposure, rainfall, retail price and school day and holiday flags. Training runs to 30 June 2020 and the final 98 days are held out for testing.
2. **Exploration:** classical decomposition shows a weekly swing of about 15 GWh, ADF, KPSS and Ljung-Box tests confirm seasonal non-stationarity, and the ACF has spikes at lags 7, 14, 21 and 28. Demand has a U-shaped relationship with maximum temperature, and holidays sit around 20 GWh below normal days.
3. **Models compared:**
   - seasonal naive (same day last week)
   - Holt-Winters exponential smoothing
   - regression with ARIMA errors, using maximum temperature, its square, holiday and school day as predictors
   - dynamic harmonic regression, using the same predictors plus three Fourier pairs for the weekly cycle in place of a seasonal ARIMA term
4. **Diagnostics:** the chosen model's residuals pass the Ljung-Box test for white noise, with the residual ACF inside the confidence bands.

## Limitations

- The test period falls during the COVID-19 lockdown, so demand patterns differ from anything in the training data.
- Forecasts use the observed test-period temperatures, so they are conditional on knowing the weather.
- Prediction intervals widen considerably at the 98-day horizon.

## Running it

Requires R with the `forecast`, `fpp2` and `tseries` packages, and a LaTeX install (such as TinyTeX) to render the PDF.

The dataset is not included. Place `elecDaily.csv` in a `data/` folder next to `report.Rmd`, then knit the report in RStudio or run:

```r
rmarkdown::render("report.Rmd")
```

## Files

- `report.Rmd`: the full analysis and report source
- `report.pdf`: the rendered report
