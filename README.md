Empirical Asset Pricing: Fama-French 3-Factor AnalysisOverview
This repository contains an empirical asset pricing analysis estimating factor loadings for individual equities using the Fama-French 3-Factor Model.The project implements an end-to-end data pipeline in Python that retrieves monthly adjusted equity returns, pulls official factor return series from the Kenneth R. French Data Library, estimates an Ordinary Least Squares (OLS) multi-factor regression, and evaluates risk exposures and abnormal returns ($\alpha$).Econometric FrameworkThe model decomposes an asset's excess return into systematic market risk, size premium, and value premium:$$R_{i,t} - R_{f,t} = \alpha_i + \beta_{Mkt}(R_{m,t} - R_{f,t}) + \beta_{SMB} \text{SMB}_t + \beta_{HML} \text{HML}_t + \epsilon_{i,t}$$Variables$R_{i,t} - R_{f,t}$: Excess return of the target asset over the risk-free rate ($R_f$).$R_{m,t} - R_{f,t}$ (Mkt-RF): Market excess return (equity risk premium).$\text{SMB}_t$ (Small Minus Big): Size premium measuring the return spread of small-cap stocks over large-cap stocks.$\text{HML}_t$ (High Minus Low): Value premium measuring the return spread of high book-to-market stocks over low book-to-market (growth) stocks.$\alpha_i$ (Jensen's Alpha): Risk-adjusted abnormal return unexplained by the three systematic factors.$\epsilon_{i,t}$: Idiosyncratic error term.Data & MethodologyAsset Selection: Apple Inc. (AAPL) monthly adjusted returns across a 5-year sample period (2019-01-01 to 2024-01-01).Data Sources:Equity price series fetched via yfinance API, adjusted for stock splits and dividend distributions.Monthly factor series (Mkt-RF, SMB, HML, RF) queried directly from the Kenneth French Data Library via pandas-datareader.Statistical Estimation:Temporal alignment via monthly period indexing (pd.PeriodIndex).OLS estimation via statsmodels.api with standard errors and t-statistics computed for all coefficients.Key Empirical FindingsMarket Exposure ($\beta_{Mkt}$): Statistically significant positive coefficient ($\beta > 1$), indicating greater sensitivity to systematic market fluctuations than the aggregate index.Size Exposure ($\beta_{SMB}$): Statistically significant negative coefficient, reflecting strong negative loading on the small-firm premium consistent with large-cap profile.Value Exposure ($\beta_{HML}$): Negative coefficient on the High-Minus-Low factor, documenting growth-oriented return characteristics over the sample window.Abnormal Return ($\alpha$): Evaluates whether the asset generated statistically significant alpha after controlling for systematic multi-factor risks.VisualizationsFigure 1: Cumulative Excess Return vs. Market Excess Performance over the 5-year sample window.

Repository Structure
├── fama_french_analysis.ipynb   # Main Jupyter Notebook containing data ingestion, OLS model, and plots
├── performance_plot.png         # Cumulative return comparison chart
└── README.md                    # Project overview, theoretical framework, and empirical summary

Setup & Execution
Prerequisites
Python 3.9+
Required libraries:
pip install pandas numpy yfinance pandas-datareader statsmodels matplotlib

Running the Analysis
Open the notebook in Jupyter or Google Colab:

jupyter notebook fama_french_analysis.ipynb

References
Fama, E. F., & French, K. R. (1993). Common risk factors in the returns on stocks and bonds. Journal of Financial Economics, 33(1), 3-56.

Kenneth R. French Data Library: Dartmouth Tuck Data Library
