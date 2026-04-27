# ETF Analysis & Mystery Allocation Reverse Engineering

A quantitative analysis pipeline for a universe of anonymised ETFs, benchmarked against main asset classes, with a focus on identifying the composition of two unknown "Mystery Allocations" (MA1 and MA2) through statistical and optimisation methods.



## Overview

This project explores a dataset of anonymised ETFs alongside a set of named main asset classes (equities, fixed income, commodities, etc.) and two mystery portfolio allocations. The analysis proceeds in four stages:

1. **Exploratory Data Analysis** - characterise the ETF universe and benchmark assets
2. **Performance & Risk Metrics** - compute and visualise return, volatility, Sharpe/Sortino ratios, and drawdowns
3. **Relational Analysis** - correlation heatmaps, OLS factor regression, and PCA-based attribution
4. **Mystery Allocation Identification** - reverse-engineer MA1 (static) and MA2 (dynamic) using constrained optimisation



## Libraries used


pandas
numpy
matplotlib
seaborn
cvxpy
scikit-learn
statsmodels


## Project Structure

### 1. Data Preparation
Loads and aligns all four datasets to a common date range
Checks for and reports missing values
Identifies and removes outlier ETFs (`ETF 72`, `ETF 64`, `ETF 48`, `ETF 46`) based on z-score visualisation

### 2. Exploratory Data Analysis
Z score plots of main asset prices (equities vs. other assets) and anonymised ETF prices
Key statistics table for main asset classes: annualised return, volatility, Sharpe ratio, max drawdown, skewness, kurtosis
Descriptive statistics for the ETF universe

### 3. Performance & Risk Metrics
Computed on log returns for both the ETF universe and main assets:

Cumulative log returns (time series + distribution)
Risk adjusted cumulative returns
Annualised return and volatility distributions
Risk return scatter plot
Sharpe and Sortino ratio distributions and comparison scatter
Top/bottom 5 ETFs by total return, volatility, and risk adjusted metrics

### 4. Relational Analysis
**Correlation heatmap** between ETFs and main asset classes, sorted by S&P 500 correlation
**OLS regression** of each ETF's returns on all main asset class returns; significant betas are extracted and plotted
**PCA** on main asset returns: scree plot, loadings chart (first 5 PCs), PCA based return attribution per ETF

### 5. Mystery Allocation Identification

**MA1 : Static allocation:**
Solved using convex optimisation (CVXPY), minimises tracking error between MA1 and a long only, fully invested combination of ETFs.

**MA2 : Dynamic allocation:**
Uses a rolling 252 day window (1 year) to solve the same constrained least squares problem at each step, producing a time varying implied weight series. Dominant regimes are identified visually and a fitted reconstruction is compared to the original MA2 series.

**Goodness of fit metrics** (correlation, R², annualised RMSE, annualised tracking error) are reported for both models.



## Key Outputs

Z-score comparison charts
Asset class performance bar chart
ETF cumulative return and distribution plots
Risk-return and drawdown scatter plots
Sharpe vs. Sortino scatter with 45° reference line
Correlation and regression coefficient heatmaps
PCA scree plot, loadings chart, and return attribution bar chart
Rolling implied weight chart for MA2
Filtered dominant regime allocation chart
Fitted vs. original MA2 cumulative return comparison



## ExtraInfo

Log returns are used throughout (ln(P_t / P_{t-1})), which implies cumulative returns are additive sums rather than compounded products.
The Sortino ratio uses a non standard calculation (downside deviation computed on annualised returns rather than daily returns) we have intended the results obtained to be comparatively within this project rather than against external benchmarks.
The dynamic optimisation loop is computationally intensive (~500+ rolling windows).
