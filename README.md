# alpha-corporate-social

A corporate social factor constructed from two complementary academic replications: employee satisfaction (Edmans 2011) and socially stigmatized industries (Hong & Kacperczyk 2009). The project builds a transparent, reproducible pipeline linking the Great Place to Work "100 Best" lists to CRSP/Compustat identifiers, replicates the original asset-pricing results, and extends both samples through 2025.

## Research Hypothesis

Firms with high employee satisfaction earn abnormal returns because equity markets systematically undervalue intangible human capital (Edmans 2011). Conversely, socially stigmatized "sin" stocks (alcohol, tobacco, gaming) earn a premium because institutional investors avoid them due to social norms, creating an excess return for unconstrained investors (Hong & Kacperczyk 2009). Combining these two signals — long employee-satisfaction firms, short sin stocks — produces a Corporate Social Factor that captures both sides of the social norm premium.

## Data Sources

All raw data files are stored locally and excluded from version control.

| Source | WRDS Table / Dataset | Description |
|---|---|---|
| CRSP | `crsp.msf`, `crsp.msenames` | Monthly stock returns, prices, shares outstanding |
| CRSP | `crsp.msedelist` | Delisting returns (Shumway 1997 adjustment) |
| Compustat | `comp.company` | Firm names, SIC, NAICS for matching and industry classification |
| CRSP/CCM | `crsp.ccmxpf_lnkhist` | Compustat gvkey → CRSP permno point-in-time linking |
| Fama-French | `ff.fivefactors_monthly`, `ff.factors_monthly` | MKT-RF, SMB, HML, RMW, CMA, MOM (monthly) |
| Great Place to Work | Web (BeautifulSoup scraping) | Annual "100 Best Companies to Work For" lists (2001–2025) |

### `Lists/` folder
Contains historical list files (HTML/DOCX) sourced from EBSCO (2001–2015) and other archives, used to reconstruct pre-web-era rankings.

## Pipeline Overview

### `Fortune100.ipynb` — List Construction & Company Matching
Scrapes the annual Great Place to Work / Fortune "100 Best" rankings and links public companies to Compustat GVKEYs and CRSP PERMNOs:
1. Web scraping via `requests` + `BeautifulSoup`
2. Name normalization (remove legal suffixes, punctuation, parentheses)
3. Fuzzy matching via `rapidfuzz` token-sort against Compustat name pool (WRDS)
4. Manual GVKEY overrides for known edge cases (spin-offs, brand vs. legal names)
5. Explicit exclusion of private companies and foreign subsidiaries
6. Warm-start matching: use later-year matches to bootstrap earlier years

### `great_place_to_work_portfolio.ipynb` — Edmans (2011) Replication
Replicates **Portfolio II** from Edmans (2011):
- **Universe:** 70 publicly traded US companies from the 1998 "100 Best" list (68 at formation + Steelcase IPO Mar 1998 + Goldman IPO Jun 1999)
- **Holding period:** February 1998 – December 2009 (143 months), buy-and-hold
- **Portfolio construction:** Equal-weighted and value-weighted monthly returns; delisting-adjusted via `(1+RET)*(1+DLRET)−1`; −30% assumption for performance-related delistings with missing DLRET
- **Benchmarks:** CAPM, Carhart 4-factor (MKT-RF, SMB, HML, MOM), industry-adjusted (FF48 industry portfolios), all with Newey-West HAC standard errors
- **Extension:** Sample extended through 2025 using the full annual GPTW panel

### `price_of_sin.ipynb` — Hong & Kacperczyk (2009) Replication
Replicates the main return tests from Hong & Kacperczyk (2009):
- **Sin stocks:** Alcohol (SIC 2080–2085), Tobacco (SIC 2100–2199), Gaming (NAICS 7132xx, 72112x via Compustat segments)
- **Comparable stocks:** FF48 industries Food (#1), Soda (#3), Fun (#7), Meals & Hotels (#43)
- **Long-short spread:** SIN − COMP, equal- and value-weighted
- **Factor models:** CAPM → Carhart 4-factor → FF5, all with Newey-West HAC standard errors
- **Extensions:** Rolling 36-month alpha, Fama-MacBeth cross-sectional regressions, standalone SIN and COMP portfolio alphas
- **Sample period:** 1965–2025 (extended from the original 1965–2006)

## Key Results

- **Edmans replication:** The 1998 GPTW portfolio generates positive abnormal returns (Carhart 4-factor alpha) over the 1998–2009 holding period, consistent with Table 4 Panel B of the original paper.
- **Hong & Kacperczyk replication:** Sin stocks earn a significant return premium over comparable stocks after controlling for market, size, value, and momentum factors.
- **Extension:** Both effects persist in the extended samples through 2025.

## Requirements

- **WRDS account** with access to CRSP, Compustat, and Fama-French tables
- Python 3.9+
- Key packages: `pandas`, `numpy`, `statsmodels`, `wrds`, `matplotlib`, `requests`, `beautifulsoup4`, `rapidfuzz`

## How to Run

1. Ensure WRDS credentials are configured (`~/.pgpass` or environment variables)
2. Run `Fortune100.ipynb` to build or update the GPTW company panel
3. Run `great_place_to_work_portfolio.ipynb` for the Edmans replication
4. Run `price_of_sin.ipynb` for the Hong & Kacperczyk replication

## References

- Edmans, A. (2011). Does the Stock Market Fully Value Intangibles? Employee Satisfaction and Equity Prices. *Journal of Financial Economics*, 101(3), 621–640.
- Hong, H., & Kacperczyk, M. (2009). The Price of Sin: The Effects of Social Norms on Markets. *Journal of Financial Economics*, 93(1), 15–36.
- Shumway, T. (1997). The Delisting Bias in CRSP Data. *Journal of Finance*, 52(1), 327–340.
- Carhart, M. M. (1997). On Persistence in Mutual Fund Performance. *Journal of Finance*, 52(1), 57–82.
- Fama, E. F., & French, K. R. (2015). A Five-Factor Asset Pricing Model. *Journal of Financial Economics*, 116(1), 1–22.
