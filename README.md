# Corporate Social Factor Development

This repository is part of an ongoing research project aimed at developing a **Corporate Social Factor** suitable for inclusion alongside **JKP-style factors** (Jensen–Kelly–Pedersen).  
The project focuses on constructing social-related return predictors using **transparent data construction**, **academic replication**, and **extended sample periods**.

The factor is built from **two complementary components**, each grounded in well-known academic literature.

---

## 1. Great Place to Work – Company Matching to Compustat (US)

Inspired by **Edmans (2011)**, *“Does the Stock Market Fully Value Intangibles? Employee Satisfaction and Equity Prices”*, this part of the project constructs a clean mapping between the **Great Place to Work / Fortune 100 Best Companies to Work For** lists and standard financial databases.

### What is done

- Web scraping of the annual *Great Place to Work – 100 Best Companies* rankings
- Careful normalization and cleaning of company names
- Explicit identification and exclusion of **private companies**
- Matching of public firms to **Compustat GVKEYs** and **CRSP identifiers**
- Manual overrides for known edge cases (spin-offs, brand names, legal entities)
- Construction of a **year-consistent panel** using warm-start matching from later years

This approach avoids common pitfalls in name matching and ensures that **only valid public firms** enter the asset-pricing sample.

### Research goal

- Replicate the main results of **Edmans (2011)** using a modern data pipeline
- Extend the sample period through **2025**  
- Use the annual GPTW list to construct a **long-only corporate social factor**

---

## 2. The Price of Sin – Industry-Based Social Norms

The second component follows **Hong & Kacperczyk (2009)**,  
*“The Price of Sin: The Effects of Social Norms on Markets”*.

### What is done

- Identification of “sin” industries using **Fama–French industry classifications**
- Mapping industries to Compustat and CRSP securities
- Construction of portfolios concentrated in sin stocks
- Evaluation of excess returns relative to standard risk factors

### Research goal

- Replicate the core findings of **Hong & Kacperczyk (2009)**
- Extend the sample period beyond the original paper
- Test whether sin stocks continue to earn **abnormal excess returns**, consistent with a social-norm-related risk premium
- Integrate this component with the employee-satisfaction factor into a broader **Corporate Social Factor**

---

## Final Objective

The ultimate goal is to combine:

- **Employee satisfaction (positive social capital)**  
- **Socially stigmatized industries (negative social norms)**  

into a unified, well-documented **Corporate Social Factor** that can be:

- Evaluated in standard asset-pricing tests
- Compared to existing JKP-style factors
- Shared in a fully reproducible research pipeline

---

## Status

- ✅ Data construction and matching pipeline completed for recent years  
- 🔄 Backfilling earlier years using warm-start matching  
- ⏳ Portfolio construction and factor testing in progress  

---

## References

- Edmans, A. (2011). *Does the Stock Market Fully Value Intangibles? Employee Satisfaction and Equity Prices.*
- Hong, H., & Kacperczyk, M. (2009). *The Price of Sin: The Effects of Social Norms on Markets.*

---

## Disclaimer

This project is research-oriented.  
All classifications, matches, and overrides are documented and reflect best judgment at the time of construction.
