# Walmart Retail Sales — EDA v2 & v3
 
**45 stores · 6,435 weekly records · Feb 2010 – Oct 2012 · $6.7B total revenue**
 
This repo contains two upgraded notebooks built on top of my internship EDA (v1).
v2 replaces SQLite + static charts with PostgreSQL and interactive Plotly visuals,
and adds feature engineering. v3 builds directly on v2's engineered features to
train and compare three ML sales forecasting models.
 
---
 
## Why this upgrade exists
 
The internship version (v1) answered 7 predefined SQL questions and produced
static Seaborn charts. It was a good start but had three real gaps:
 
1. **SQLite** — not what production data teams use. PostgreSQL introduces real
   connection management, type casting (`::numeric`), and window functions
   (`RANK()`, `LAG()`, `NTILE()`) that don't exist in SQLite.
2. **Static charts** — a PNG screenshot isn't an analysis tool. Plotly charts
   let you zoom, hover for exact values, and filter — useful for actual decision-making.
3. **Raw features only** — the original columns (Temperature, Fuel_Price, CPI,
   Unemployment) are weak predictors in isolation. Lag features and rolling
   averages unlock the temporal structure in the data, which v3's ML models need.
---
 
## Key Findings
 
### v2 — EDA + SQL
 
| Finding | Detail |
|---|---|
| **Store 20 leads all 45 stores** | $301.4M total vs Store 33 at $37.2M — an 8× gap. The 52% coefficient of variation across stores is unusually high and suggests location-specific factors (store size, demographics) dominate over economic variables. |
| **Holiday weeks average +7.8% lift** | $1.12M vs $1.04M non-holiday. But the lift is not evenly distributed — December is the strongest month ($1.28M avg) while January is the weakest ($924K avg), a $358K swing. |
| **YoY revenue grew +7.0% (2010→2011)** | $2.29B → $2.45B, confirmed via `LAG()` window function. 2012 is partial (cuts off October) so not directly comparable. |
| **Fuel price shows weak negative correlation** | Higher fuel prices associate slightly with lower sales, but the effect is inconsistent across store tiers — high-volume stores are less sensitive than low-volume ones. |
| **CPI quartile analysis (NTILE)** | Sales are highest in the 2nd CPI quartile, not the lowest — suggesting Walmart shoppers are less price-sensitive than the CPI relationship might imply in isolation. |
 
### v3 — ML Forecasting
 
| Finding | Detail |
|---|---|
| **Gradient Boosting wins** | Best MAPE and R² across all 45 stores. Lag features and rolling averages are its top predictors — more important than any economic variable. |
| **Ridge Regression is the honest baseline** | Strong at trend-following, weak on holiday spikes. Useful for showing what a linear model can and cannot do. |
| **Lag_1 is the single strongest predictor** | Last week's sales explains more variance than Temperature, CPI, and Unemployment combined. Temporal structure dominates economic structure in this dataset. |
| **8-week forecast** | Rolling forecast for any store — each week's predicted value feeds the next week's lag feature. |
 
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
 
## Dataset
 
**Walmart Store Sales** — 45 US retail stores, weekly sales data, Feb 2010–Oct 2012.  
6,435 rows · 8 columns (Store, Date, Weekly_Sales, Holiday_Flag, Temperature, Fuel_Price, CPI, Unemployment)
 
Source: [Kaggle — Walmart Sales Dataset](https://www.kaggle.com/datasets/mikhail1681/walmart-sales)
