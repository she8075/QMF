# Counterfactual Inflation Analysis: What If Ukraine Had Been Part of the Euro Area?

**Quantitative Methods in Finance**  
**M2 Finance, Technology & Data — Université Paris 1 Panthéon-Sorbonne**  
**Lecturer:** Eric Vansteenberghe (Banque de France & Paris 1)

---

## Overview

This project constructs a counterfactual inflation path for Ukraine under the hypothesis that it had been a member of the Euro Area since 2006, using two complementary econometric methods:

**Primary — SVAR Bayoumi-Eichengreen (1993) / Blanchard-Quah (1989)**  
Bivariate structural VARs `[d_log_IPI, d_YoY_inflation]` estimated on 2006–2021 for Ukraine and the Euro Area separately. The Blanchard-Quah long-run restriction identifies supply vs. demand shocks. The counterfactual replaces Ukraine's structural demand shocks with EA demand shocks via IRF convolution (H=24), keeping Ukrainian supply shocks unchanged. This operationalises "being in the Euro Area" precisely: ECB demand policy replaces NBU demand policy; geopolitical and energy supply shocks remain Ukrainian.

**Complement — Ciccarelli-Mojon (2010) factor model**  
Extracts the EA common inflation factor from an 11-country HICP panel (PCA fitted on 2006–2019 only, projected onto 2006–2025). Ukraine's loading λ=0.683 is estimated on the 2018–2021 stable IT window, the only period of genuine NBU monetary sovereignty with statistically significant co-movement with the EA factor. The instability of λ across other windows is itself evidence that structural identification via SVAR is required. The factor model captures the total treatment effect, including exchange-rate and credibility channels that the SVAR allocates to supply shocks by construction.

The counterfactual is consistent with the monetary sovereignty chronology established in Part A: treatment intensity is time-varying, near-zero during dollar-peg periods (+0.3pp SVAR) and maximum during the 2014–2015 devaluation episode (+3.4pp SVAR / +24.5pp CM).

---

## Repository Structure

```
├── ukraine_counterfactual.ipynb        # Main notebook (fully reproducible, Restart & Run All)
├── part_a_final_corrected.md           # Part A: chronology table + Deliverable 2
├── output/
│   ├── 06_svar_shocks_diagnostic.png   # Structural shocks diagnostic (demand + supply)
│   ├── 07_counterfactual_final.png     # Main deliverable figure
│   └── 08_summary.txt                  # Gap summary and methodological notes
├── data/
│   ├── data_ecb_hicp_panel.csv         # EA HICP panel (ECB, 11 countries, 2000–2025)
│   ├── data_ukraine_cpi_raw.csv        # Ukraine CPI MoM index (SSSU)
│   └── data_ukraine_ipi_imf.csv        # Ukraine IPI (IMF Production Indexes)
└── README.md
```

---

## Reproducing the Results

### Requirements

```bash
pip install pandas numpy matplotlib statsmodels scikit-learn requests
```

### Running

1. Clone the repository
2. Run `ukraine_counterfactual.ipynb` from top to bottom (`Restart & Run All`)
3. All external data is downloaded programmatically — no manual downloads required

The Euro Area IPI is fetched directly from the Eurostat SDMX 2.1 REST API:
```
https://ec.europa.eu/eurostat/api/dissemination/sdmx/2.1/data/STS_INPR_M/M.PRD.B-D.SCA.I15.EA20/
```

---

## Data Sources

| Dataset | Source | Access |
|---|---|---|
| Ukraine CPI (MoM index) | State Statistics Service of Ukraine (SSSU) via SDMX | Course repository |
| EA HICP panel (11 countries) | ECB Data Portal | Course repository |
| Ukraine IPI | IMF Production Indexes | Course repository |
| Euro Area IPI (EA20, B-D, SCA, 2015=100) | Eurostat STS_INPR_M | Programmatic download |
| UAH/USD exchange rate | National Bank of Ukraine API | Programmatic download |
| Henry Hub gas price | FRED — MHHNGSP | Programmatic download |
| Wheat price | FRED — PWHEAMTUSDM | Programmatic download |

---

## Key Methodological Choices

| Choice | Rationale |
|---|---|
| SVAR as primary method | Exam requires SVAR/LP as core; structural identification via long-run restrictions is more defensible than window-fitted factor loading |
| PCA on 2006–2019 only | Prevents 2022 energy shock (HICP >10% EA-wide) from dominating the first principal component |
| VAR estimated on 2006–2021 | Avoids structural break from war-driven IPI collapse in Donbas post-Feb. 2022 |
| EA IPI (Eurostat, EA20, B-D, SCA) as output | Blanchard-Quah requires real activity variable — not a price factor |
| IRF convolution H=24 (not cumsum) | Respects natural shock dissipation; cumsum produces artificial trend drift |
| λ estimated on 2018–2021 | Only window with significant co-movement (p=0.000); λ instability across other windows justifies SVAR primacy |

---

## Key Results

| Period | SVAR gap (pp) | CM gap (pp) | Interpretation |
|---|---|---|---|
| 2006–2013 (peg era) | +0.3 | +1.2 | Quasi-null: dollar-to-euro anchor substitution |
| 2008–09 (GFC) | +2.0 | +12.0 | SVAR: moderate demand differential; CM: devaluation pass-through |
| 2014–2015 (Maidan/Crimea) | +3.4 | +24.5 | SVAR shows demand not primary driver; CM captures total exchange-rate cost |
| 2016–2021 (IT era) | −1.5 | +2.6 | NBU more restrictive than ECB: credibility built at output cost |

The CM vs SVAR contrast in 2014–2015 (+24.5pp vs +3.4pp) is itself an analytical result: the dominant driver of the inflation spike was the exchange-rate and supply channel, not excess domestic demand, confirmed by the SVAR's supply shock classification.

The SVAR gap of −1.5pp in 2016–2021 is the most analytically rich finding: the NBU ran tighter monetary conditions than any EA member would have experienced under ECB policy (ZLB, QE, TLTRO), building credibility independently at the cost of compressing domestic demand — the Giavazzi-Pagano (1988) mechanism in reverse.

---

## References

- Astrov, V. & Podkaminer, L. (2017). *Ukraine: Selected Economic Issues.* wiiw Policy Notes No. 19.
- Barro, R.J. & Gordon, D.B. (1983). Rules, Discretion and Reputation. *Journal of Monetary Economics*, 12(1).
- Bayoumi, T. & Eichengreen, B. (1993). Shocking aspects of European monetary integration. Cambridge University Press.
- Blanchard, O.J. & Quah, D. (1989). The dynamic effects of aggregate demand and supply disturbances. *American Economic Review*, 79(4).
- Calvo, G.A. & Reinhart, C.M. (2002). Fear of Floating. *Quarterly Journal of Economics*, 117(2).
- Ciccarelli, M. & Mojon, B. (2010). Global inflation. *Review of Economics and Statistics*, 92(3).
- De Grauwe, P. (2012). The governance of a fragile eurozone. *Australian Economic Review*, 45(3).
- Giavazzi, F. & Pagano, M. (1988). The advantage of tying one's hands. *European Economic Review*, 32(5).
- IMF Country Report No. 15/69 — Ukraine: EFF Request, March 2015.
- IMF Country Report No. 16/320 — Ukraine: Ex Post Evaluation, September 2016.
- Kilian, L. & Lütkepohl, H. (2017). *Structural Vector Autoregressive Analysis.* Cambridge University Press.
- Mundell, R.A. (1961). A Theory of Optimum Currency Areas. *American Economic Review*, 51(4).
