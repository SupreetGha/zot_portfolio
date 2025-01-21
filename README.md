# Hypothesis Testing for Portfolio Performance

## Table of Contents
- [Data Preparation](#data-preparation)
- [Hypothesis Testing](#hypothesis-testing)
- [Findings](#findings)
- [Limitations](#limitations)

### Project Overview
---
This project evaluates the performance of two investment portfolios, ZOT and ANT, using hypothesis testing. The aim is to analyze daily returns, test the equality of mean returns, and assess the variance to inform investment strategies.

### Data Source(s)
- Yahoo Finance API: Historical stock prices for both portfolios.

### Tools
  - Python: Data Analysis and Visualization
  - Libraries: Pandas, NumPy, Matplotlib, Scipy, Statsmodels

### Data Preparation

#### Portfolio Setup

Loaded data for the ZOT and ANT portfolios:
```python
import pandas as pd
import numpy as np
import yfinance as yf
from pandas_datareader import data as pdr
import scipy.stats as sst
import statsmodels.api as sm
yf.pdr_override()

# ZOT Portfolio
ZOT = ["ENPH", "SEDG", "JPM", "V", "JNJ", "EW", "UNH", "MRK", "JLL", "CBRE", "VWAGY"]
ZOT_prices = yf.download(ZOT, start="2015-1-1")['Adj Close'].dropna()

# ANT Portfolio
ANT = ["PRTS", "ACMR", "VTNR", "FDP", "VSEC", "CLFD", "ZIP", "IHRT"]
ANT_prices = yf.download(ANT, start="2015-1-1")['Adj Close'].dropna()
```
#### Calculated daily returns and average returns:
```python
ZOT_returns = ZOT_prices.pct_change().mean(axis=1)
ANT_returns = ANT_prices.pct_change().mean(axis=1)

ZOT_returns.name = "ZOT Daily Returns"
ANT_returns.name = "ANT Daily Returns"
```
### Hypothesis Testing
#### Daily Mean Return for ZOT portfolio
Null Hypothesis: The mean daily return of the ZOT portfolio equals zero.
```python
result = sst.ttest_1samp(ZOT_returns.dropna(), popmean=0)
print(result)
```
#### Comparison of Mean Returns Between Portfolios
Null Hypothesis: The mean daily return of the ZOT portfolio equals zero.
```python
result = sst.ttest_ind(ZOT_returns.dropna(), ANT_returns.dropna())
print(result)
```
#### Variance Analysis of Returns
Null Hypothesis: Variances of daily returns for ZOT and ANT portfolios are equal.
```python
f_stat = ZOT_returns.var() / ANT_returns.var()
critical_value = sst.f.ppf(q=0.95, dfn=len(ZOT_returns)-1, dfd=len(ANT_returns)-1)
print(f"F-Statistic: {f_stat}, Critical Value: {critical_value}")
```
### Findings
#### Daily Returns
- ZOT Portfolio: The null hypothesis was rejected, indicating significant non-zero daily mean returns.
- ANT Portfolio: Failed to reject the null hypothesis when comparing mean returns to a specified value.
#### Comparisons
- Mean Returns: No significant difference between ZOT and ANT daily mean returns.
- Variances: Rejected the null hypothesis, indicating unequal variances.
### Limitations
- Data spans specific periods and may not capture all market conditions.
- Portfolios contain different industries, introducing inherent biases.


