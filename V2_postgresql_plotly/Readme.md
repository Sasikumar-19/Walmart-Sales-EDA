## Walmart Retail Sales — EDA v2

**Upgrade:** SQLite → PostgreSQL · Seaborn (static) → Plotly (interactive) · Raw columns → Feature-engineered dataset
This is the v2 upgrade of the internship EDA project. The dataset, businessquestions, and core findings are the same — but the tooling, SQL depth, andanalytical output are significantly stronger.
 
---
## v1 vs v2 — What Changed
 
| Area | v1 (Internship) | v2 (This notebook) |
|---|---|---|
| Database | SQLite | PostgreSQL via SQLAlchemy |
| Credentials | Hardcoded | `.env` file — safe to push to GitHub |
| Charts | Seaborn static PNGs | Plotly interactive (hover, zoom, export) |
| SQL depth | Basic `SELECT`, `GROUP BY`, `WHERE` | `RANK()`, `LAG()`, `DATE_TRUNC()`, `CORR()`, `EXTRACT()`, `STDDEV()` |
| Features | Raw columns only | 10 engineered features (lags, rolling avg, holiday dummies) |
| Output | Static notebook | Exportable charts + feature-engineered CSV pushed back to Postgres |
 
---
### Interactive Plotly Charts (5 charts)
 
All charts are fully interactive — hover for exact values, zoom, pan, download as PNG.
 
| Chart | Type | What it shows |
|---|---|---|
| Store Revenue Ranking | Horizontal bar | All 45 stores sorted by total sales, top store highlighted |
| Monthly Sales Trend | Line with markers | Revenue over time with holiday weeks marked in red |
| Holiday vs Normal Distribution | Box plot | Full distribution comparison — median, IQR, outliers |
| External Factors Heatmap | Heatmap | Correlation of Temperature, Fuel, CPI, Unemployment vs sales |
| Year-over-Year Comparison | Grouped bar | 2010 vs 2011 monthly comparison side by side |
 
---
 
### Feature Engineering (10 new columns)
 
All features engineered **per store** (using `groupby` + `transform`) so that
Store A's lag values never bleed into Store B's rows.
 
| Feature | Description | Business purpose |
|---|---|---|
| `lag_1` | Sales from 1 week ago | Short-term momentum signal |
| `lag_2` | Sales from 2 weeks ago | Momentum confirmation |
| `lag_4` | Sales from 4 weeks ago | ~1 month ago |
| `lag_52` | Sales from same week last year | Year-over-year seasonal baseline |
| `rolling_4_mean` | 4-week rolling average | Smoothed trend, removes noise |
| `rolling_8_mean` | 8-week rolling average | Medium-term trend |
| `rolling_4_std` | 4-week rolling std deviation | Volatility signal |
| `week_over_week_change` | % change vs prior week | Momentum direction |
| `sales_vs_store_avg` | Sales vs that store's historical mean | Store-normalised performance |
| `Is_Holiday_*` | 4 specific holiday dummies (Super Bowl, Labor Day, Thanksgiving, Christmas) | Individual holiday effect vs generic flag |
 
**Key correlation insight:** `lag_1` and `rolling_4_mean` have the strongest
correlation with `weekly_sales` among all engineered features — making this
dataset forecasting-ready (used directly in EDA v3).
 
---
## Tech Stack
Python · PostgreSQL · SQLAlchemy · psycopg2 · Plotly · Pandas · NumPy · python-dotenv · Jupyter
 
## Related
- [v1 — Original internship EDA](https://github.com/Sasikumar-19/Walmart-Sales-EDA/blob/main/V1_Original/README.md)
- [v3 — ML Forecasting (built on v2's engineered features)](../v3_ml_forecasting/README.md)
