# Time Series Analysis

A small collection of quantitative time-series projects focused on **statistical dependence, persistence, stationarity, decomposition, and forecasting**.

The repository currently contains a working implementation of the **Hurst exponent using Rescaled Range (R/S) analysis** and a second, broader **Time Series Analysis (TSA)** project that is currently under development.

## Projects

| Project | Status | Main topics |
|---|---|---|
| **Hurst Exponent — R/S Analysis** | Implemented | Long-range dependence, persistence, anti-persistence, log-log regression |
| **Time Series Analysis — SARIMA Forecasting** | 🚧 TBD / In progress | Decomposition, stationarity, ACF/PACF, ARIMA/SARIMA, diagnostics, forecasting |

---

## 1. Hurst Exponent — Rescaled Range Analysis

The first project implements the **Hurst exponent** \(H\) using the classical **Rescaled Range (R/S)** approach.

The Hurst exponent is commonly used as a diagnostic of the dependence structure of a time series:

- \(H < 0.5\) — **anti-persistent** behaviour; movements tend to reverse more often,
- \(H \approx 0.5\) — behaviour consistent with a process without clear long-range dependence,
- \(H > 0.5\) — **persistent** behaviour; movements tend to be followed by movements in the same direction.


### Methodology

For each selected window length \(n\), the implementation:

1. splits the input series into non-overlapping blocks of length \(n\),
2. subtracts the block mean,
3. computes cumulative deviations,
4. calculates the range

   \[
   R(n)=\max(Y_k)-\min(Y_k),
   \]

5. scales the range by the sample standard deviation \(S(n)\),
6. averages \(R/S\) across blocks of the same length,
7. estimates the slope of the log-log relationship

   \[
   \log(R/S)_n = c + H\log(n)+\varepsilon_n.
   \]

The estimated slope is the Hurst exponent:

\[
\hat H = \hat\beta_1.
\]


### Example usage

```python
H = hurst_rs(daily_returns)
print(f"Estimated Hurst exponent: {H:.3f}")
```

Potential extensions:

- regression summary using `statsmodels`,
- confidence intervals / bootstrap inference for \(H\),
- log-log diagnostic plot,
- configurable overlapping vs. non-overlapping windows,
- comparison across prices, returns, absolute returns, volatility proxies, and spreads.

---

## 2. Time Series Analysis — Decomposition & SARIMA

> **Status: TBD / under development**

The second project will be an end-to-end time-series modelling and forecasting workflow built around a real-world series.

The objective is to move from **raw observations to a statistically validated forecasting model**, with particular emphasis on decomposition, stationarity and seasonal dynamics.



#### Time-series decomposition

The series will be decomposed using STL-decomposition.


#### Stationarity analysis

The project will examine whether the series is stationary and determine the transformations required before modelling.

Planned diagnostics include:

- visual inspection of rolling behaviour,
- **ADF test**,
- **KPSS test**,
- transformations such as logarithms,
- regular and seasonal differencing.

#### Dependence structure

- **ACF** analysis,
- **PACF** analysis,
- identification of plausible autoregressive and moving-average orders,
- inspection of seasonal lags.

#### ARIMA / SARIMA modelling

Candidate models will be specified in the form

\[
ARIMA(p,d,q)
\]

and, for seasonal data,

\[
SARIMA(p,d,q)(P,D,Q)_s.
\]

Model selection will consider statistical fit together with out-of-sample forecasting performance.

#### Model diagnostics

Planned residual diagnostics include:

- residual time-series plots,
- residual ACF,
- Ljung-Box test,
- distributional checks,
- verification that the remaining errors behave approximately like white noise.

#### Forecast evaluation

The final models will be evaluated using a chronological train/test split and, where useful, rolling or expanding-window validation.

Potential metrics:

- MAE,
- RMSE,
- MAPE / sMAPE,
- forecast interval coverage.

The final section will compare candidate specifications and present the selected model together with forecasts and uncertainty intervals.

---

## Tech Stack

- **Python**
- **NumPy**
- **pandas**
- **Matplotlib**
- **statsmodels** *(planned for TSA and Hurst inference/diagnostics)*

 