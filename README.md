# Awesome Financial Data APIs [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated, actively maintained list of free and paid financial data APIs, Python client libraries, and data quality tools for individual investors, quants, and developers — with verified availability status as of 2026.

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0_1.0-lightgrey.svg)](https://creativecommons.org/publicdomain/zero/1.0/)
![Last Verified](https://img.shields.io/badge/Last%20Verified-2026--04-green)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)

**Why this list exists**: After Yahoo Finance's unofficial API became unreliable in 2023 and Quandl transitioned to Nasdaq Data Link (with many free datasets removed), the financial data landscape fragmented significantly. This list consolidates what actually works in 2026, with honest notes on rate limits, data quality, and cost.

---

## Table of Contents

- [Quick Comparison](#quick-comparison)
- [Stock Price & Market Data](#stock-price--market-data)
- [Fundamental & Financial Statement Data](#fundamental--financial-statement-data)
- [Macroeconomic & Government Data](#macroeconomic--government-data)
- [Options & Derivatives Data](#options--derivatives-data)
- [Alternative Data](#alternative-data)
- [Python Client Libraries](#python-client-libraries)
- [Multi-Source Platforms](#multi-source-platforms)
- [Data Validation & Quality Tools](#data-validation--quality-tools)
- [Tutorials & Recipes](#tutorials--recipes)
- [Contributing](#contributing)

---

## Quick Comparison

| API | Free Tier | Real-Time | Fundamentals | Macro | Python Client | Status (2026) |
|-----|-----------|-----------|--------------|-------|---------------|---------------|
| Alpha Vantage | ✅ 25 req/day | ❌ (15-min delay) | ✅ | ✅ | ✅ | 🟢 Active |
| Polygon.io | ✅ Limited | ✅ (paid) | ✅ (paid) | ❌ | ✅ | 🟢 Active |
| FRED (St. Louis Fed) | ✅ Unlimited | N/A | ❌ | ✅ | ✅ | 🟢 Active |
| FXMacroData | ✅ USD no-key access | N/A | ❌ | ✅ | ✅ | 🟢 Active |
| yfinance (Yahoo) | ✅ Unofficial | ❌ | ✅ | ❌ | ✅ | 🟡 Unstable |
| SEC EDGAR | ✅ Unlimited | N/A | ✅ | ❌ | ✅ | 🟢 Active |
| Tiingo | ✅ 500 req/hour | ❌ | ✅ | ❌ | ✅ | 🟢 Active |
| IEX Cloud | ✅ Limited | ✅ (paid) | ✅ (paid) | ❌ | ✅ | 🟢 Active |
| Nasdaq Data Link | ✅ Some datasets | N/A | ✅ (paid) | ✅ | ✅ | 🟢 Active |
| OpenBB Platform | ✅ Aggregator | Depends | Depends | Depends | ✅ | 🟢 Active |
| Bloomberg Terminal | ❌ Paid only | ✅ | ✅ | ✅ | ✅ | 🟢 Institutional |

---

## Stock Price & Market Data

### Free Tier Available

**[Alpha Vantage](https://www.alphavantage.co/)**
- **Free**: 25 requests/day, 500/month; intraday data with 15-min delay
- **Data**: OHLCV, adjusted prices, splits, dividends, forex, crypto
- **Python**: `pip install alpha_vantage`
- **Note**: Free tier is genuinely usable for research and backtesting with daily data. Paid plans start at $50/month for higher limits.

```python
from alpha_vantage.timeseries import TimeSeries
ts = TimeSeries(key='YOUR_API_KEY', output_format='pandas')
data, meta = ts.get_daily_adjusted('AAPL', outputsize='full')
```

---

**[Polygon.io](https://polygon.io/)**
- **Free**: Unlimited historical data (prior-day close), no real-time
- **Data**: US stocks, options, forex, crypto; excellent data quality
- **Python**: `pip install polygon-api-client`
- **Note**: The free tier's historical data coverage is exceptional. Real-time requires a paid plan ($29/month+). Actively maintained with good documentation.

```python
from polygon import RESTClient
client = RESTClient(api_key="YOUR_API_KEY")
aggs = client.get_aggs("AAPL", 1, "day", "2023-01-01", "2024-01-01")
```

---

**[Tiingo](https://www.tiingo.com/)**
- **Free**: 500 requests/hour, up to 1,000 unique symbols/day
- **Data**: End-of-day prices, adjusted for splits and dividends; news feed
- **Python**: `pip install tiingo` or via `pandas-datareader`
- **Note**: Often overlooked, but Tiingo's data quality is consistently high and their free tier is genuinely generous. Good for portfolio research.

```python
from tiingo import TiingoClient
client = TiingoClient({'api_key': 'YOUR_KEY'})
prices = client.get_dataframe('AAPL', startDate='2020-01-01', endDate='2024-01-01')
```

---

**[yfinance](https://github.com/ranaroussi/yfinance)** (Yahoo Finance wrapper)
- **Free**: No official API key required
- **Data**: OHLCV, dividends, splits, financials, holders, options chains
- **Python**: `pip install yfinance`
- **Warning (2026)**: Yahoo Finance's backend has changed repeatedly since 2023. `yfinance` maintainers actively patch breaking changes, but expect occasional outages. Do not rely on this for production systems. For research and backtesting, it remains the most convenient option.

```python
import yfinance as yf
ticker = yf.Ticker("AAPL")
hist = ticker.history(period="5y")
info = ticker.info  # fundamentals, but structure changes frequently
```

---

**[Stooq](https://stooq.com/)**
- **Free**: Unlimited via pandas-datareader
- **Data**: US, European, and Asian equities; indices; commodities
- **Python**: `pip install pandas-datareader`

```python
import pandas_datareader as pdr
data = pdr.get_data_stooq('AAPL', start='2020-01-01', end='2024-01-01')
```

---

### Paid (Institutional/Professional)

**[Bloomberg Data License / B-PIPE](https://www.bloomberg.com/professional/product/data-license/)**
- Industry standard for institutional-grade data. Expensive (several thousand USD/month). Accessed via `blpapi` Python library. Only relevant if you have Bloomberg Terminal access.

**[Refinitiv Eikon / LSEG Data Platform](https://www.lseg.com/en/data-analytics)**
- Bloomberg's main competitor. Python access via `refinitiv-data` library.

**[FactSet](https://www.factset.com/)**
- Strong for fundamental data, estimates, and consensus data. Python SDK available.

---

## Fundamental & Financial Statement Data

**[SEC EDGAR Full-Text Search & XBRL API](https://efts.sec.gov/LATEST/search-index?q=%22financial+statements%22&dateRange=custom&startdt=2023-01-01)**
- **Free**: Completely free, maintained by the U.S. Securities and Exchange Commission
- **Data**: All SEC filings (10-K, 10-Q, 8-K), structured XBRL financial data
- **Python**: `pip install sec-api` (third-party wrapper) or use the official EDGAR APIs directly
- **Note**: The official EDGAR XBRL API (`data.sec.gov/api/xbrl/`) provides structured financial data for all public U.S. companies. This is the authoritative source for financial statements.

```python
import requests
# Get all 10-K filings for Apple
url = "https://data.sec.gov/submissions/CIK0000320193.json"
resp = requests.get(url, headers={"User-Agent": "your@email.com"})
filings = resp.json()

# Get structured XBRL financial data
xbrl_url = "https://data.sec.gov/api/xbrl/companyfacts/CIK0000320193.json"
xbrl = requests.get(xbrl_url, headers={"User-Agent": "your@email.com"}).json()
# Access EPS: xbrl['facts']['us-gaap']['EarningsPerShareBasic']
```

---

**[Simfin](https://simfin.com/)**
- **Free**: Income statement, balance sheet, cash flow for US equities (bulk download)
- **Python**: `pip install simfin`
- **Note**: Excellent for academic research and backtesting fundamental strategies. Data lags by a quarter but is freely bulk-downloadable.

```python
import simfin as sf
sf.set_api_key('free')  # Free key for basic access
sf.set_data_dir('~/simfin_data/')
income = sf.load_income(variant='annual', market='us')
```

---

**[Macrotrends](https://www.macrotrends.net/)** — Scraped via unofficial means; not an API. Use SEC EDGAR instead for production.

**[Nasdaq Data Link (formerly Quandl)](https://data.nasdaq.com/)**
- **Free**: Select datasets (Zillow, Wiki Prices EOD discontinued)
- **Paid**: Premium financial databases (Sharadar, FRED Premium, etc.)
- **Python**: `pip install nasdaq-data-link`

---

## Macroeconomic & Government Data

**[FRED (Federal Reserve Economic Data)](https://fred.stlouisfed.org/)**
- **Free**: 800,000+ economic time series. Rate limit: 1,000 requests/day per API key (free key)
- **Data**: GDP, CPI, unemployment, interest rates, yield curve, money supply, housing data
- **Python**: `pip install fredapi`
- **Note**: The gold standard for US macroeconomic data. Essential for any macro-aware investment strategy.

```python
from fredapi import Fred
fred = Fred(api_key='YOUR_FREE_KEY')

# 10-Year Treasury Yield
treasury_10y = fred.get_series('GS10')

# Yield Curve (10Y - 2Y spread) - recession predictor
spread = fred.get_series('T10Y2Y')

# CPI All Items
cpi = fred.get_series('CPIAUCSL')
```

Key FRED Series for Investors:
| Series ID | Description |
|-----------|-------------|
| `SP500` | S&P 500 Index |
| `GS10` | 10-Year Treasury Yield |
| `T10Y2Y` | 10Y-2Y Treasury Spread (Yield Curve) |
| `CPIAUCSL` | CPI All Urban Consumers |
| `UNRATE` | Unemployment Rate |
| `FEDFUNDS` | Federal Funds Rate |
| `M2SL` | M2 Money Supply |
| `HOUST` | Housing Starts |
| `VIXCLS` | CBOE Volatility Index (VIX) |
| `DCOILWTICO` | WTI Crude Oil Price |

---

**[Bureau of Labor Statistics (BLS) API](https://www.bls.gov/developers/)**
- **Free**: 500 queries/day (unregistered); 2,500/day with free registration
- **Data**: CPI components, PPI, employment by sector, wage growth

```python
import requests, json
headers = {'Content-type': 'application/json'}
data = json.dumps({"seriesid": ["CUUR0000SA0", "CWUR0000SA0"], "startyear": "2020", "endyear": "2026"})
resp = requests.post('https://api.bls.gov/publicAPI/v2/timeseries/data/', data=data, headers=headers)
```

---

**[FXMacroData](https://fxmacrodata.com/documentation/reference)**
- **Free**: Catalogue and release-calendar endpoints require no key; USD macro history supports up to 100 no-key requests/day
- **Data**: Macroeconomic indicators, FX rates, release calendars, central-bank decisions, COT positioning, commodities, and bond-yield context
- **Python**: `pip install fxmacrodata`
- **Note**: Verified 2026-08 with Python 3.12 and `fxmacrodata` 1.2.1 against the public USD catalogue, calendar, and inflation endpoints. FX rates and other protected datasets require API-key access.

```python
from fxmacrodata import Client

calendar = Client().get_calendar("usd")
```

---

**[World Bank Open Data](https://data.worldbank.org/)**
- **Free**: Global economic indicators for 200+ countries
- **Python**: `pip install wbgapi`

```python
import wbgapi as wb
# GDP per capita for US, China, Germany
gdp = wb.data.DataFrame('NY.GDP.PCAP.CD', ['USA', 'CHN', 'DEU'], time=range(2000, 2026))
```

---

**[U.S. Census Bureau API](https://www.census.gov/data/developers/about.html)**
- **Free**: Economic indicators, retail sales, housing data

---

## Options & Derivatives Data

**[CBOE Options Data](https://datashop.cboe.com/)**
- **Free**: End-of-day VIX data, some historical options data
- **Paid**: Comprehensive historical options chains

**[Polygon.io Options](https://polygon.io/docs/options)**
- **Free tier**: Limited; paid plans include full options chains with Greeks

**[yfinance Options Chains](https://github.com/ranaroussi/yfinance)**
- **Free**: Near-real-time options chains (unofficial)

```python
import yfinance as yf
ticker = yf.Ticker("SPY")
expirations = ticker.options  # List of expiration dates
opt = ticker.option_chain('2024-12-20')
calls = opt.calls
puts = opt.puts
```

---

## Alternative Data

**[NewsAPI](https://newsapi.org/)**
- **Free**: 100 requests/day for developers
- **Data**: Financial news headlines and articles

**[Reddit API (via PRAW)](https://www.reddit.com/dev/api/)**
- **Free**: Access to Reddit posts and comments (r/wallstreetbets, r/investing sentiment)
- **Python**: `pip install praw`

**[Google Trends (via pytrends)](https://github.com/GeneralMills/pytrends)**
- **Free**: Unofficial wrapper for Google Trends
- **Use case**: Retail investor attention proxy, ticker search volume

```python
from pytrends.request import TrendReq
pytrends = TrendReq()
pytrends.build_payload(['NVDA', 'AMD'], timeframe='today 12-m')
interest = pytrends.interest_over_time()
```

---

## Python Client Libraries

| Library | Purpose | Install | Status (2026) |
|---------|---------|---------|---------------|
| `yfinance` | Yahoo Finance wrapper | `pip install yfinance` | 🟡 Unstable but widely used |
| `pandas-datareader` | Multi-source data reader | `pip install pandas-datareader` | 🟡 Some sources deprecated |
| `fredapi` | FRED official wrapper | `pip install fredapi` | 🟢 Stable |
| `alpha_vantage` | Alpha Vantage client | `pip install alpha_vantage` | 🟢 Stable |
| `polygon-api-client` | Polygon.io official | `pip install polygon-api-client` | 🟢 Stable |
| `simfin` | SimFin fundamentals | `pip install simfin` | 🟢 Stable |
| `tiingo` | Tiingo client | `pip install tiingo` | 🟢 Stable |
| `wbgapi` | World Bank data | `pip install wbgapi` | 🟢 Stable |
| `pytrends` | Google Trends | `pip install pytrends` | 🟡 Unofficial |
| `openbb` | Multi-source aggregator | `pip install openbb` | 🟢 Active |

---

## Multi-Source Platforms

**[OpenBB Platform](https://openbb.co/)**
- **Free**: Open-source, self-hostable financial data terminal
- **Concept**: One Python interface to 100+ data providers; you bring your own API keys
- **Python**: `pip install openbb`

```python
from openbb import obb
obb.user.credentials.alpha_vantage_api_key = "YOUR_KEY"
# Fetch from multiple providers with a unified interface
data = obb.equity.price.historical("AAPL", provider="polygon")
macro = obb.economy.fred_series(symbol="GS10", provider="fred")
```

**[financedatabase](https://github.com/JerBouma/FinanceDatabase)**
- **Free**: Database of 300,000+ financial instruments with metadata
- **Use case**: Discovering tickers by sector, country, market cap

---

## Data Validation & Quality Tools

**Common data quality issues to check for:**
- Dividend/split adjustment gaps
- Survivorship bias in historical datasets
- Point-in-time vs. as-reported data for backtesting
- Missing trading day handling

```python
import pandas as pd

def check_data_quality(df: pd.DataFrame, ticker: str) -> dict:
    """Basic data quality checks for OHLCV data."""
    issues = {}

    # Check for missing dates (non-trading days excluded)
    issues['null_count'] = df.isnull().sum().to_dict()

    # Check for price outliers (>20% single-day move)
    daily_returns = df['Close'].pct_change()
    issues['extreme_moves'] = df[daily_returns.abs() > 0.20].index.tolist()

    # Check for zero volume days
    issues['zero_volume_days'] = len(df[df['Volume'] == 0])

    # Check high >= low
    issues['bad_hloc'] = len(df[df['High'] < df['Low']])

    return issues
```

---

## Tutorials & Recipes

### Recipe 1: Build a Free Multi-Source Price Fetcher

```python
import yfinance as yf
import requests
from fredapi import Fred

def get_price_with_fallback(ticker: str, start: str, end: str):
    """Fetch prices with yfinance, fall back to Tiingo if it fails."""
    try:
        data = yf.download(ticker, start=start, end=end, auto_adjust=True)
        if data.empty:
            raise ValueError("Empty data from yfinance")
        return data, "yfinance"
    except Exception as e:
        print(f"yfinance failed ({e}), trying Tiingo...")
        # Tiingo fallback (requires TIINGO_API_KEY env var)
        import os
        from tiingo import TiingoClient
        client = TiingoClient({'api_key': os.environ['TIINGO_API_KEY']})
        data = client.get_dataframe(ticker, startDate=start, endDate=end)
        return data, "tiingo"
```

### Recipe 2: Download Yield Curve Data from FRED

```python
from fredapi import Fred
import pandas as pd
import matplotlib.pyplot as plt

fred = Fred(api_key='YOUR_FREE_KEY')

maturities = {
    '3M': 'DTB3', '6M': 'DTB6', '1Y': 'DGS1',
    '2Y': 'DGS2', '5Y': 'DGS5', '10Y': 'DGS10', '30Y': 'DGS30'
}

yield_data = pd.DataFrame({
    label: fred.get_series(series_id)
    for label, series_id in maturities.items()
})

# Plot current yield curve
latest = yield_data.iloc[-1].dropna()
plt.figure(figsize=(10, 5))
plt.plot([0.25, 0.5, 1, 2, 5, 10, 30][:len(latest)], latest.values, 'bo-')
plt.title(f"U.S. Treasury Yield Curve — {yield_data.index[-1].date()}")
plt.xlabel("Maturity (Years)")
plt.ylabel("Yield (%)")
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig("yield_curve.png", dpi=150)
```

### Recipe 3: Bulk Download SEC EDGAR XBRL Data

```python
import requests
import pandas as pd

def get_company_facts(cik: str) -> dict:
    """Fetch all XBRL financial facts for a company from SEC EDGAR."""
    cik_padded = str(cik).zfill(10)
    url = f"https://data.sec.gov/api/xbrl/companyfacts/CIK{cik_padded}.json"
    resp = requests.get(url, headers={"User-Agent": "research@yourproject.com"})
    resp.raise_for_status()
    return resp.json()

def extract_metric(facts: dict, metric: str, form: str = "10-K") -> pd.Series:
    """Extract a specific XBRL metric (e.g. 'EarningsPerShareBasic') as a time series."""
    try:
        data = facts['facts']['us-gaap'][metric]['units']['USD']
    except KeyError:
        return pd.Series(dtype=float)
    df = pd.DataFrame(data)
    df = df[df['form'] == form].drop_duplicates('end')
    df['end'] = pd.to_datetime(df['end'])
    return df.set_index('end')['val'].sort_index()

# Example: Get Apple's net income history
apple_facts = get_company_facts(320193)  # Apple's CIK
net_income = extract_metric(apple_facts, 'NetIncomeLoss')
print(net_income.tail(10))
```

---

## Rate Limit Cheat Sheet

| API | Free Rate Limit | Notes |
|-----|----------------|-------|
| Alpha Vantage | 25 req/day, 5/min | Use `time.sleep(12)` between calls |
| FRED | 1,000 req/day | Bulk downloads not counted |
| Polygon.io (free) | Unlimited for EOD | 5 req/min for historical |
| SEC EDGAR | 10 req/sec | Requires User-Agent header |
| BLS | 500 req/day (unregistered) | Register for 2,500/day |
| yfinance | Unofficial, no documented limit | Rotate user agents for bulk use |

---

## Data Sources Comparison by Use Case

| Use Case | Best Free Option | Best Paid Option |
|----------|-----------------|-----------------|
| Backtesting US stocks (EOD) | Tiingo or Stooq | Polygon.io or Refinitiv |
| Fundamental analysis | SEC EDGAR XBRL | FactSet or Intrinio |
| Macro research | FRED | Bloomberg |
| Options chains | yfinance (unstable) | Polygon.io |
| Real-time prices | IEX Cloud (free tier) | Bloomberg |
| Global equities | yfinance | Refinitiv |
| Earnings estimates | None (free) | FactSet or Refinitiv |

---

## Status Legend

- 🟢 Active — maintained, documented, reliable
- 🟡 Unstable — works but may break without notice; not recommended for production
- 🔴 Deprecated — no longer functional or maintained

*Status last verified: April 2026. Open a PR if any status has changed.*

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Key guidelines:
- Verify the API/library still works before submitting
- Include free tier limitations
- Note if data requires academic affiliation or is geographically restricted

---

## License

[CC0 1.0 Universal](LICENSE) — This list is public domain. Use it freely.

*This repository is maintained by independent researchers and is not affiliated with any of the listed services.*
