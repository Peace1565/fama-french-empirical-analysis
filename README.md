# Empirical Asset Pricing: Fama-French 3-Factor Analysis

**Author:** Peace Augustine  
**Focus:** Empirical Asset Pricing & Quantitative Finance  

---

## 1. Overview
This repository implements an empirical asset pricing analysis estimating factor loadings and risk-adjusted abnormal performance ($\alpha$) for individual equities using the **Fama-French 3-Factor Model** (Fama & French, 1993).

The project constructs an automated Python data pipeline that pulls monthly adjusted equity returns, retrieves official empirical factor series directly from the **Kenneth R. French Data Library**, executes an Ordinary Least Squares ($\text{OLS}$) multi-factor regression, and provides econometric diagnostic evaluations of risk exposures.

---

## 2. Econometric Framework
The Fama-French 3-Factor Model extends the standard Capital Asset Pricing Model ($\text{CAPM}$) by augmenting market risk with size and value risk factors:

$$R_{i,t} - R_{f,t} = \alpha_i + \beta_{\text{Mkt}}(R_{m,t} - R_{f,t}) + \beta_{\text{SMB}}\text{SMB}_t + \beta_{\text{HML}}\text{HML}_t + \epsilon_{i,t}$$

### Variable Definitions
* **$R_{i,t} - R_{f,t}$ (Asset Excess Return):** Monthly return of the target equity asset over the one-month Treasury bill rate ($R_f$).
* **$R_{m,t} - R_{f,t}$ ($\text{Mkt-RF}$):** Broad market excess return, capturing systematic equity risk premium.
* **$\text{SMB}_t$ (Small Minus Big):** Size factor measuring the historic return spread of small-cap equities over large-cap equities.
* **$\text{HML}_t$ (High Minus Low):** Value factor measuring the return spread of high book-to-market (value) firms over low book-to-market (growth) firms.
* **$\alpha_i$ (Jensen's Alpha):** Average abnormal return unexplained by systematic exposures to market, size, and value factors.
* **$\epsilon_{i,t}$:** Idiosyncratic error term satisfying standard Gauss-Markov assumptions ($\mathbb{E}[\epsilon_{i,t}] = 0, \, \text{Var}(\epsilon_{i,t}) = \sigma^2$).

---

## 3. Data & Pipeline Architecture

[ Yahoo Finance API ]           [ Kenneth French Data Library ]
            │                                     │
    (Equity Returns)                      (Factor Returns)
            │                                     │
            └───────────► [ pandas ] ◄────────────┘
                             │
                  (Monthly Period Index)
                             │
                 [ statsmodels OLS Engine ]
                             │
            ┌────────────────┴────────────────┐
            ▼                                 ▼
   [ Factor Betas & t-stats ]       [ Cumulative Return Plots ]

---
## Data Sources
* **Asset Data:** Monthly adjusted closing prices for Apple Inc. (`AAPL`, 2019–2024) retrieved via `yfinance`.
* **Factor Benchmarks:** Monthly Fama-French 3-Factor series (`Mkt-RF`, `SMB`, `HML`, `RF`) imported directly from the Kenneth French Data Library.

---
## Estimation Routine

Converted timestamps into standardized monthly frequency buckets (pd.PeriodIndex(freq='M')).
Calculated logarithmic/percentage monthly returns for the underlying equity.
Aligned equity returns with published factor series and computed excess returns ($R_{i,t} - R_{f,t}$).
Estimated the multi-factor linear equation using statsmodels.api.OLS with heteroskedasticity-robust standard errors.

---

### 4. Estimated Factor Loadings

| Parameter | Factor Exposure | Interpretation & Empirical Significance |
| :--- | :--- | :--- |
| **$\beta_{\text{Mkt}}$** | **Market Risk** ($\text{Mkt-RF}$) | $\beta > 1.0$ ($p < 0.01$). Statistically significant positive loading, indicating higher cyclical sensitivity than the broad market benchmark. |
| **$\beta_{\text{SMB}}$** | **Size Premium** ($\text{SMB}$) | $\beta < 0$ ($p < 0.01$). Strongly negative and statistically significant loading, reflecting large-cap firm dynamics that move inversely to small-firm premia. |
| **$\beta_{\text{HML}}$** | **Value Premium** ($\text{HML}$) | $\beta < 0$ ($p < 0.05$). Negative factor loading, confirming strong growth-stock orientation (low book-to-market). |
| **$\alpha$** | **Jensen's Alpha** | Evaluates risk-adjusted abnormal return unexplained by the three systematic factors. |

### Key Empirical Findings
* **Market Exposure ($\beta_{\text{Mkt}} > 1.0, p < 0.01$):** Statistically significant positive loading, showing higher cyclical volatility than the aggregate market.
* **Size Exposure ($\beta_{\text{SMB}} < 0, p < 0.01$):** Significant negative loading, confirming Apple's large-cap profile.
* **Value Exposure ($\beta_{\text{HML}} < 0, p < 0.05$):** Negative loading on the value factor, reflecting growth characteristics.
* **Jensen's Alpha ($\alpha$):** Quantifies excess return after controlling for market, size, and value factor premia.

---
## 5. Repository Structure

fama-french-empirical-analysis/
fama_french_analysis.ipynb   # Main Jupyter/Colab notebook with end-to-end data pipeline & OLS models
performance_plot.png         # Cumulative return vs. benchmark factor visualizations
README.md                    # Project documentation, theoretical derivations, and results

---
## 6. Replication & Setup
Environment Requirements

Python 3.9+
Core dependencies:

pip install pandas numpy yfinance pandas-datareader statsmodels matplotlib

Running the Analysis in Google Colab or Jupyter
Open Google Colab or your local Jupyter environment.
Upload and open fama_french_analysis.ipynb.
Run all cells sequentially to ingest data and produce the regression tables and charts.

---
## 7. References

Fama, E. F., & French, K. R. (1993). Common risk factors in the returns on stocks and bonds. Journal of Financial Economics, 33(1), 3–56.
French, K. R. (2024). Data Library: Fama/French 3 Factors [Daily & Monthly]. Dartmouth Tuck School of Business.



