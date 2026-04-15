# CLAUDE.md — alpha-corporate-social

## What This Project Is
A Corporate Social Factor pipeline with two components: (1) replication and extension of Edmans (2011) using annual Great Place to Work "100 Best" lists matched to CRSP/Compustat, and (2) replication and extension of Hong & Kacperczyk (2009) identifying sin stocks (alcohol, tobacco, gaming) and computing long-short return spreads vs. comparable industries. Both pipelines use WRDS for financial data and Newey-West HAC regressions for factor spanning.

## Python Stack
- `pandas`, `numpy` — data manipulation
- `wrds` — WRDS database access (CRSP, Compustat, Fama-French)
- `requests`, `beautifulsoup4` — web scraping of Great Place to Work annual lists
- `rapidfuzz` — fuzzy name matching (sponsor/company name → Compustat gvkey)
- `statsmodels` — OLS, Newey-West HAC standard errors, Fama-MacBeth regressions
- `matplotlib` — charting

## Notebooks (run in this order)
1. `Fortune100.ipynb` — scrape GPTW lists and build company → gvkey → permno panel
2. `great_place_to_work_portfolio.ipynb` — Edmans (2011) Portfolio II replication
3. `price_of_sin.ipynb` — Hong & Kacperczyk (2009) replication

## Data Sources
Raw data lives **locally only** and is never committed to version control:
- CRSP monthly returns and delisting data pulled live from WRDS
- Compustat company table pulled live from WRDS
- Fama-French factors pulled live from WRDS
- `Lists/` — historical HTML/DOCX list files for pre-web GPTW years (committed, small)

## Files to NEVER Commit
*.parquet, *.csv, *.xlsx, *.png, *.jpg, *.pdf, *.svg, .env
Data/, data/, figures/, output/, Old/, .venv/

## Coding Conventions
- Vectorized operations only — no `iterrows()` or Python loops over DataFrames
- All date alignment to month-end via `pd.offsets.MonthEnd(0)` before merging
- Delisting adjustment: `(1+RET)*(1+DLRET)−1`; use −30% for performance delistings with missing DLRET
- Newey-West lags: use `int(np.ceil(T**(1/3)))` or 6 lags for monthly data
- WRDS CCM linkage: always use `linktype IN ('LU','LC')` and `linkprim IN ('P','C')`

## Session Start
1. Open `Fortune100.ipynb` if you need to update or extend the GPTW list panel
2. Open `great_place_to_work_portfolio.ipynb` for the Edmans replication
3. Open `price_of_sin.ipynb` for the Hong & Kacperczyk replication
4. WRDS connection is established inline — requires `~/.pgpass`
