# 📘 Abdullah Al-Othaim Markets — Comprehensive Step-by-Step Guide

> **Project:** Stock Performance Analytics & Interactive Dashboard  
> **Ticker:** 4001.SR (Tadawul / Saudi Exchange)  
> **Dataset Range:** March 4, 2010 → January 29, 2026  
> **Author:** Hassan Jameel — United Group  
> **Last Updated:** April 2026

---

## 📌 Table of Contents

1. [Project Overview](#1-project-overview)
2. [Prerequisites & Environment Setup](#2-prerequisites--environment-setup)
3. [Step 1: Data Acquisition](#3-step-1-data-acquisition)
4. [Step 2: Data Loading & Inspection](#4-step-2-data-loading--inspection)
5. [Step 3: Data Cleaning & Preprocessing](#5-step-3-data-cleaning--preprocessing)
6. [Step 4: Feature Engineering](#6-step-4-feature-engineering)
7. [Step 5: Exploratory Data Analysis (EDA)](#7-step-5-exploratory-data-analysis-eda)
8. [Step 6: Interactive Dashboard Creation](#8-step-6-interactive-dashboard-creation)
9. [Step 7: Report Generation](#9-step-7-report-generation)
10. [Step 8: Saving as PDF](#10-step-8-saving-as-pdf)
11. [Project File Structure](#11-project-file-structure)
12. [Key Metrics Summary](#12-key-metrics-summary)
13. [Challenges & Solutions](#13-challenges--solutions)
14. [Next Steps & Recommendations](#14-next-steps--recommendations)
15. [FAQ](#15-faq)

---

## 1. Project Overview

This project performs a **comprehensive Exploratory Data Analysis (EDA)** on the historical stock data of **Abdullah Al-Othaim Markets Company** — the leading Saudi hypermarket chain listed on the Tadawul stock exchange.

### Goals:
- ✅ Clean and preprocess raw financial time-series data
- ✅ Engineer technical indicators (SMA, Bollinger Bands, RSI, MACD)
- ✅ Conduct statistical tests (normality, stationarity, correlation)
- ✅ Build 20+ interactive Plotly visualizations
- ✅ Generate an interactive multi-page dashboard
- ✅ Create a standalone printable report
- ✅ Document findings for Kaggle publication

---

## 2. Prerequisites & Environment Setup

### Required Software
| Tool | Version | Purpose |
|------|---------|---------|
| Python | 3.9+ | Core programming language |
| Jupyter Notebook | Latest | Interactive EDA environment |
| Web Browser | Chrome/Edge | View dashboard & report |

### Required Python Libraries
```bash
pip install pandas numpy matplotlib seaborn plotly scipy
```

### Key Libraries Used
| Library | Purpose |
|---------|---------|
| `pandas` | Data loading, cleaning, feature engineering |
| `numpy` | Numerical computations |
| `plotly` | Interactive chart creation |
| `seaborn` | Statistical visualizations |
| `matplotlib` | Static plotting |
| `scipy` | Statistical tests (Shapiro-Wilk, ADF) |

---

## 3. Step 1: Data Acquisition

### Source
The raw data was downloaded from **Yahoo Finance** using the ticker symbol `4001.SR`.

**Download link:** [finance.yahoo.com/quote/4001.SR/history](https://finance.yahoo.com/quote/4001.SR/history)

### File Details
| Property | Value |
|----------|-------|
| Filename | `Abdullah Al-Othaim Markets Company.csv` |
| Records | 3,910 rows |
| Columns | 6 (Date, Open, High, Low, Close, Volume) |
| File Size | ~250 KB |
| Format | Comma-separated values (CSV) |

### How to Download Manually
1. Visit [Yahoo Finance](https://finance.yahoo.com/quote/4001.SR/history)
2. Set the date range: `Mar 4, 2010` → `Jan 29, 2026`
3. Click **"Download"** to save the CSV file
4. Place it in your project directory

---

## 4. Step 2: Data Loading & Inspection

### Load the Data
```python
import pandas as pd

df = pd.read_csv("Abdullah Al-Othaim Markets Company.csv")
print(f"Shape: {df.shape}")
print(df.head())
print(df.info())
print(df.describe())
```

### Key Observations During Inspection
- **Column encoding issue:** The Date column header appears as `??` due to encoding artifacts
- **Data types:** Prices are `float64`, Volume is `int64`
- **No null values** in the core OHLCV columns
- **Date range:** 2010-03-04 to 2026-01-29

---

## 5. Step 3: Data Cleaning & Preprocessing

### Fix Column Names
```python
# Rename the encoded date column
df.columns = ['Date', 'Close', 'High', 'Low', 'Open', 'Volume']

# Parse dates
df['Date'] = pd.to_datetime(df['Date'])
df = df.sort_values('Date').reset_index(drop=True)
```

### Handle Missing Data
```python
# Check for nulls
print(df.isnull().sum())

# Forward-fill any gaps (if applicable)
df = df.fillna(method='ffill')
```

### Set Date as Index
```python
df.set_index('Date', inplace=True)
```

---

## 6. Step 4: Feature Engineering

### Add Technical Indicators
```python
# Daily Returns
df['Daily_Return'] = df['Close'].pct_change()

# Moving Averages
df['SMA_50'] = df['Close'].rolling(window=50).mean()
df['SMA_200'] = df['Close'].rolling(window=200).mean()

# Bollinger Bands
df['BB_Upper'] = df['SMA_50'] + (df['Close'].rolling(50).std() * 2)
df['BB_Lower'] = df['SMA_50'] - (df['Close'].rolling(50).std() * 2)

# Daily Range (Volatility Proxy)
df['Daily_Range'] = df['High'] - df['Low']
df['Range_Pct'] = df['Daily_Range'] / df['Close'] * 100

# Cumulative Return
df['Cumulative_Return'] = (1 + df['Daily_Return']).cumprod()

# Year & Month for grouping
df['Year'] = df.index.year
df['Month'] = df.index.month
df['Day_of_Week'] = df.index.day_name()
```

---

## 7. Step 5: Exploratory Data Analysis (EDA)

The full EDA is conducted in `New_App.ipynb`. Here are the key analysis categories:

### 7.1 Overview Charts
- **Stock Price with Moving Averages** — Close price with SMA-50/200 overlays and range slider
- **Trading Volume Over Time** — Color-coded by year

### 7.2 Distribution Analysis
- **Daily Return Histogram** — With normal distribution overlay
- **Volume Distribution** — Log-scale density plot
- **QQ-Plot** — To assess normality of returns

### 7.3 Composition Analysis
- **Yearly Return Breakdown** — Stacked bar chart
- **Monthly Seasonality Heatmap** — Average returns by month/year

### 7.4 Relationship Analysis
- **Correlation Heatmap** — Between OHLCV features
- **Price vs Volume Scatter** — With trend line
- **Lag Correlation** — Autocorrelation function (ACF)

### 7.5 Comparison Analysis
- **Year-over-Year Performance** — Normalized price comparison
- **Day-of-Week Returns** — Box plot by weekday

### 7.6 Statistical Analysis
- **Descriptive Statistics Table** — Count, mean, std, quartiles
- **Hypothesis Testing** — Shapiro-Wilk normality test, ADF stationarity test
- **Rolling Volatility** — 30-day rolling standard deviation

### 7.7 Dashboard Panel
- **Candlestick Chart** — Interactive OHLC chart
- **Animated Bubble Chart** — Yearly Return vs Volume with dynamic year slider

---

## 8. Step 6: Interactive Dashboard Creation

The interactive dashboard (`dashboard.html`) was generated using `build_dashboard.py`.

### Dashboard Structure
| Page | Icon | Contents |
|------|------|----------|
| Overview | 📊 | KPIs + Price History with MAs + Volume chart |
| Distribution | 📈 | Return distribution + Volume density + QQ-Plot |
| Composition | 🧩 | Yearly breakdown + Seasonality heatmap |
| Relationship | 🔗 | Correlation matrix + Scatter plots |
| Comparison | ⚖️ | YoY comparison + Weekday analysis |
| Statistical | 📐 | Stats table + Hypothesis tests + Rolling volatility |
| Dashboard | 🎯 | Candlestick + Animated bubble chart |

### How to View
1. Open `web/dashboard.html` in any modern web browser (Chrome, Edge, Firefox)
2. Use the sidebar navigation to switch between the 7 analytical pages
3. All charts are fully interactive — hover, zoom, pan, and download

---

## 9. Step 7: Report Generation

### Standalone Report
Open `web/report.html` in your browser for a clean, single-page analytics summary covering:
- Executive summary with KPI cards
- Dataset documentation
- Key findings
- Methodology
- Challenges
- Recommendations

---

## 10. Step 8: Saving as PDF

The standalone report (`report.html`) includes print-optimized CSS styles.

### How to Save as PDF:
1. Open `web/report.html` in **Google Chrome** or **Microsoft Edge**
2. Press **Ctrl + P** (Windows) or **Cmd + P** (Mac)
3. Set **Destination** to **"Save as PDF"**
4. Under **More Settings**, ensure:
   - Layout: Portrait
   - Paper size: A4 or Letter
   - Margins: Default
   - ☑ Background graphics (enabled)
5. Click **Save**
6. Your PDF is ready to share!

---

## 11. Project File Structure

```
Abdullah Al-Othaim Markets Company/
├── Abdullah Al-Othaim Markets Company.csv   # Raw dataset
├── Abdullah Al-Othaim logo.jpg              # Company logo
├── New_App.ipynb                            # Full EDA notebook
├── app.md                                   # Kaggle PRD documentation
├── build_dashboard.py                       # Dashboard generator script
├── build_notebook.py                        # Notebook generator script
├── dashboard.html                           # Original dashboard file
│
└── web/                                     # 📁 Web deliverables folder
    ├── dashboard.html                       # Interactive Plotly dashboard
    ├── report.html                          # Standalone analytics report
    ├── styles.css                           # External stylesheet
    ├── guide.md                             # This step-by-step guide
    └── images/
        └── logo.jpg                         # Company logo image
```

---

## 12. Key Metrics Summary

| Metric | Value |
|--------|-------|
| Total Trading Days | 3,910 |
| Date Range | Mar 4, 2010 → Jan 29, 2026 |
| Latest Close Price | 6.66 SAR |
| All-Time High | 68.54 SAR |
| Total Return | +777.5% |
| Average Daily Volume | 2,394,120 shares |
| Average Daily Volatility | 1.85% |
| Mean Close Price | 23.41 SAR |
| Median Close Price | 18.90 SAR |

---

## 13. Challenges & Solutions

| # | Challenge | Solution |
|---|-----------|----------|
| 1 | Date column encoded as `??` | Manual column rename to `Date` |
| 2 | Missing trading days (weekends/holidays) | Business-day reindexing with forward-fill |
| 3 | High market volatility masking trends | Log-returns and rolling smoothing windows |
| 4 | Large HTML dashboard size (~3.3 MB) | Plotly data embedded for offline use; deferred loading |
| 5 | Adjusted vs. nominal pricing confusion | Documented split-adjustment in metadata |

---

## 14. Next Steps & Recommendations

### Immediate Enhancements
1. **🤖 Predictive Modeling** — Apply ARIMA, Facebook Prophet, or LSTM to forecast next-day/weekly price direction
2. **📰 Sentiment Analysis** — Scrape Arabic-language financial news to correlate sentiment with stock movements
3. **📊 Peer Benchmarking** — Compare performance against BinDawood, Al-Raya, and TASI index

### Medium-Term Goals
4. **💼 Portfolio Optimization** — Build a multi-asset portfolio using Modern Portfolio Theory (MPT) and calculate the Sharpe ratio
5. **⚡ Live Data Pipeline** — Automate daily data ingestion via `yfinance` API with scheduled updates
6. **🏛️ Fundamental Data Integration** — Merge quarterly earnings, P/E, and dividend yield data for multi-factor analysis

### Advanced Projects
7. **🎲 Monte Carlo Simulation** — Estimate Value at Risk (VaR) and Conditional VaR for risk profiling
8. **🌐 Web Deployment** — Host the dashboard on GitHub Pages, Vercel, or Streamlit Cloud
9. **📱 Mobile Responsiveness** — Optimize the dashboard layout for tablet and mobile viewing
10. **🔐 API Endpoint** — Create a REST API serving the latest analytical metrics

---

## 15. FAQ

**Q: Can I update the data to include newer trading days?**  
A: Yes! Download the latest CSV from [Yahoo Finance (4001.SR)](https://finance.yahoo.com/quote/4001.SR/history), replace the existing CSV, and re-run the `build_dashboard.py` script to regenerate the dashboard.

**Q: Why is the dashboard HTML file so large (~3.3 MB)?**  
A: The file contains embedded Plotly chart data (20+ interactive charts with ~3,900 data points each). This is by design for fully offline functionality — no internet connection needed to view the charts.

**Q: How do I save the report as a PDF?**  
A: Open `web/report.html` in Chrome → Press **Ctrl+P** → Select **"Save as PDF"**. The print styles are already built in.

**Q: Can I deploy this dashboard online?**  
A: Absolutely. Upload the `web/` folder to any static hosting service:
- **GitHub Pages** — Free, version-controlled
- **Vercel** — One-click deploy with drag & drop
- **Netlify** — Free tier with custom domains

**Q: What Python version is required?**  
A: Python 3.9 or higher. Tested with Python 3.11.

---

*This guide was prepared as part of the Abdullah Al-Othaim Markets Company data analytics project.*  
*© 2026 — All rights reserved.*
