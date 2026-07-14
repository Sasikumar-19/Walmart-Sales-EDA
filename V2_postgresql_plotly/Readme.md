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
## Setup
 
### 1. Install dependencies
 
**For v2:**
```bash
pip install pandas numpy plotly sqlalchemy psycopg2-binary python-dotenv jupyter
```
 
**For v3 (additional):**
```bash
pip install scikit-learn
```
 
### 2. Connect PostgreSQL (v2)
 
Get a free Postgres instance at [Neon](https://neon.tech) or
[Supabase](https://supabase.com). Then copy `.env.example` to `.env`
and fill in your credentials:
 
```
DB_HOST=your-host.neon.tech
DB_PORT=5432
DB_NAME=walmart_db
DB_USER=your_username
DB_PASSWORD=your_password
```
 
> **No Postgres?** The notebook still runs completely. Every SQL cell
> automatically falls back to a pandas equivalent showing the same real numbers.
> Nothing is ever blank.
 
### 3. Run in order
 
```bash
# Step 1 — generates walmart_features.csv (needed by v3)
jupyter notebook v2_postgresql_plotly/EDA_v2_Walmart.ipynb
 
# Step 2 — loads walmart_features.csv, trains ML models
jupyter notebook v3_ml_forecasting/EDA_v3_ML_Forecasting.ipynb
```
 
---
 
## Version Comparison
 
| | v1 (Internship) | v2 | v3 |
|---|---|---|---|
| Database | SQLite | **PostgreSQL** | — |
| Charts | Seaborn static | **Plotly interactive** | Plotly interactive |
| SQL | 7 basic queries | **7 queries + window functions** (RANK, LAG, NTILE, CTEs) | — |
| Features | Raw columns | **+lag, rolling avg, holiday flags** | — |
| ML | None | None | **Ridge · Random Forest · Gradient Boosting** |
| Output | Notebook + PPT | Notebook + PostgreSQL tables + CSV | 8-week rolling forecast |
 
---
 
## v2 Notebook — Section Guide
 
| Section | What it covers |
|---|---|
| 1 — PostgreSQL setup | `.env` connection, SQLAlchemy engine, graceful fallback |
| 2 — Load & clean | Parse dates, fix types, push raw data to Postgres |
| 3 — SQL business queries | 7 queries using RANK(), LAG(), NTILE(), EXTRACT(), CTEs |
| 4 — Plotly visuals | Store ranking · rolling sales trend · holiday box plot · correlation heatmap · holiday lift · YoY bar |
| 5 — Feature engineering | Sales_Lag_1/2, Rolling_4wk/8wk, WoW_Change_Pct, Is_SuperBowl/LaborDay/Thanksgiving/Christmas |
| 6 — Push back to Postgres | Writes engineered dataset as `walmart_features` table + exports CSV |
| 7 — Business insights | Auto-printed summary of all key findings |
 
## v3 Notebook — Section Guide
 
| Section | What it covers |
|---|---|
| 1 — Load | Reads `walmart_features.csv` from v2 |
| 2 — Train/test split | Time-based: train 2010–2011, test 2012 (no data leakage) |
| 3 — Train 3 models | Ridge (with StandardScaler pipeline), Random Forest, Gradient Boosting |
| 4 — Model comparison | MAE, RMSE, MAPE, R² table + bar chart |
| 5 — Feature importance | Side-by-side RF vs GB importance — lag features dominate |
| 6 — 8-week forecast | Rolling forecast: predicted week feeds next week's lag |
| 7 — Summary | Final printed model results |
 
---
 
## Tech Stack
 
| Tool | Purpose |
|---|---|
| Python 3.11+ | Core language |
| pandas · NumPy | Data manipulation |
| SQLAlchemy · psycopg2 | PostgreSQL interface |
| python-dotenv | Credential management |
| Plotly | Interactive visualisations |
| scikit-learn | ML models + pipelines |
| PostgreSQL | Production database |
| Jupyter | Analysis environment |
 
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
- [v3 — ML Forecasting (built on v2's engineered features)](https://github.com/Sasikumar-19/Walmart-Sales-EDA/blob/main/V3_ml_forecasting/Readme.md)
