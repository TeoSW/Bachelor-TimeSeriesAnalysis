# 📈 Volatility Analysis and Equilibrium Relationships between Apple and Google Stocks

**🎓 Academia de Studii Economice din București**  
Faculty of Cybernetics, Statistics and Economic Informatics  
Specialization: Statistics and Economic Forecasting

**👨‍🎓 Authors:** Constantin Teodor Vasile, Danilov Matei, Nițu Vlad-Cristian, Nistor Matei  
**📅 Year:** 2025

---

## 📝 Overview

This project analyzes the volatility dynamics and potential long-run equilibrium relationships between the stock prices of **Apple (AAPL)** and **Google (GOOG)** using time series econometrics methods implemented in R.

The analysis covers the period **2006–2017**, using daily closing prices sourced from a [Kaggle dataset](https://www.kaggle.com/datasets/szrlee/stock-time-series-20050101-to-20171231) of the 30 blue-chip companies included in the Dow Jones Industrial Average (DJIA).

---

## 🎯 Objectives

- 📊 Examine the stationarity properties of log-returns for both stocks
- 📉 Model conditional volatility using GARCH(1,1)
- 🔮 Forecast short-term volatility
- 🔗 Test for cointegration between the two price series
- 📈 Estimate a VAR model on stationary returns to assess interdependence

---

## 🧮 Methodology

### 1️⃣ Log-Returns
Returns are computed as log-differences of closing prices:

```
r_t = ln(P_t / P_{t-1}) = ln(P_t) - ln(P_{t-1})
```

Log-returns are preferred over simple returns for their additivity and closer approximation to normality.

### 2️⃣ Stationarity Testing — ADF Test
The Augmented Dickey-Fuller (ADF) test is applied to log-returns. Results confirm stationarity (p-value = 0.01 for Apple), while the log-price levels are non-stationary (p-values of 0.43 and 0.28 for Apple and Google respectively). ACF/PACF analysis confirms that log-returns behave as white noise — ruling out ARMA/ARIMA models for direct return modeling.

### 3️⃣ GARCH(1,1) Model
The ARCH-LM test detects significant conditional heteroscedasticity in the residuals (χ² = 194.7, p < 2.2e-16), justifying the use of a GARCH model. The estimated GARCH(1,1) equation for Apple is:

```
σ²_t = 0.000009 + 0.080367·ε²_{t-1} + 0.896334·σ²_{t-1}
```

**📌 Key findings:**
- ✅ All parameters are statistically significant (p < 0.05)
- 📈 α₁ + β₁ ≈ 0.977 → high volatility persistence
- ✔️ Ljung-Box and ARCH-LM diagnostic tests on standardized residuals confirm the model adequately captures volatility clustering

### 4️⃣ Volatility Forecast
A 10-day ahead volatility forecast is generated from the GARCH(1,1) model. The predicted conditional standard deviation ranges between **1.35% and 1.50%**, indicating stable but elevated risk.

### 5️⃣ Cointegration Analysis (Engle-Granger)
The two log-price series are individually non-stationary (integrated of order I(1)). A linear regression is estimated and the ADF test is applied to the residuals. The residuals are non-stationary (p-value = 0.717), indicating **❌ no cointegration** between Apple and Google stock prices — i.e., no long-run equilibrium relationship exists between them.

### 6️⃣ VAR Model
Since the series are not cointegrated, a **VAR(1)** model is estimated on the stationary log-return series. Results show that Google's returns do not significantly predict Apple's returns (R² ≈ 0.04%, F-test p > 0.05), confirming the two series evolve independently.

---

## 📊 Key Results

| Analysis | Result |
|---|---|
| 📈 Log-returns stationarity (ADF) | Stationary — white noise |
| 📉 ARCH effects | Confirmed (p < 2.2e-16) |
| 🔄 GARCH(1,1) persistence (α+β) | 0.977 |
| 🔮 10-day volatility forecast | 1.35% – 1.50% |
| 🔗 Cointegration (Engle-Granger) | Not present |
| 📊 VAR(1) — cross-predictability | Not significant |

---

## 🛠️ Tools & Technologies

- 💻 **Language:** R
- 📦 **Packages:** `tseries`, `rugarch`, `vars`, `FinTS`, `ggplot2` (or similar)
- 📂 **Data source:** [Kaggle — Stock Time Series 2005–2017](https://www.kaggle.com/datasets/szrlee/stock-time-series-20050101-to-20171231)

---

## 📌 Conclusions

The log-return series of both Apple and Google are stationary and exhibit white noise behavior, consistent with weak-form market efficiency. While no autoregressive structure is present in the returns themselves, volatility clustering is clearly present and successfully captured by the GARCH(1,1) model. The absence of cointegration and the non-significant VAR results suggest that **Apple and Google stocks move independently** over this period, with no detectable long-run relationship or short-run return spillovers between them.
