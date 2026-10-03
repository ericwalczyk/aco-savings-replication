# Medicare ACO Spending Patterns – Replication (2014–2017) & Extension (2014–2024)

This project replicates and extends findings from **Muhlestein et al. (2018)**, *Medicare Accountable Care Spending Patterns: Shifting Expenditures Associated With Savings*, AJAC 6(1):11–19.

Using updated **2014–2017 MSSP ACO Public Use Files**, I recreate spending-share metrics and estimate fixed-effects models to test which expenditure shifts are associated with shared savings. I then extend the analysis through **performance year 2024**.

---

## Key Results (Replication, 2014–2017)

- **Lower inpatient and SNF spending shares** are consistently associated with higher savings.
- **Higher physician spending share** is positive but small and not statistically significant.
- The inpatient effect closely matches the original study; the SNF effect is smaller (see [Differences from the Original Study](#differences-from-the-original-study)).
- Findings hold under:
  - ACO & year fixed effects  
  - ACO‐clustered SEs  
  - Reduced models (multicollinearity)  
  - Fixed-effects logistic regression (inpatient share only; the SNF logit coefficient is not significant for 2014–2017)

**Bottom line:** ACOs that save money spend a **smaller share on inpatient and SNF care**.

The original course paper (December 2024) is in [`docs/ACO_Replication_Report.pdf`](docs/ACO_Replication_Report.pdf). It predates the current code: its savings-rate models use ACO fixed effects only with unclustered SEs, and its logit uses ACO dummy variables on all 1,569 observations, so its estimates differ from `ACO_Final.rmd`.

---

## Extension to 2024

`analysis/ACO_Extension.Rmd` re-estimates the models on every MSSP performance year from 2014 through 2024 (about 5,100 ACO-years) to test whether the pattern survived the 2019 *Pathways to Success* redesign and COVID-19. ACOs are followed across the 2018 change in CMS's ACO identifiers using CMS's official ACO ID crosswalk.

![Average spending composition of MSSP ACOs, 2014–2024](outputs/figures/spending_shares_2014_2024.png)

- **Spending kept shifting.** From 2014 to 2024 the average inpatient share fell from 30.7% to 26.5% and SNF from 7.9% to 5.6%, while outpatient rose from 18.4% to 24.5% and physician from 31.3% to 33.1%.
- **The shift happens within ACOs, not just through turnover.** Year fixed-effects models with ACO fixed effects (the original study's first model) show the same ACOs cut their inpatient share by 4.0 pp and SNF by 2.2 pp from 2014 to 2024, while outpatient rose 4.2 pp and physician 3.5 pp (all p < 0.001). Inpatient fell most sharply in 2020–2022 and has partly rebounded.
- **ACOs save far more often.** 54% generated savings in 2014 vs. 89% in 2024 (median savings rate 0.3% → 4.1%).
- **The core finding holds.** Over 2014–2024, a 1 pp higher inpatient share is associated with a 0.37 pp lower savings rate (p < 0.001), and a 1 pp higher SNF share with a 0.21 pp lower rate (p < 0.01). Both stay negative and significant when dropping the COVID years, splitting before/after 2019, and treating the pre- and post-2018 ACO IDs as separate units.
- **Hospice share also turns negative** (−0.34 pp, p < 0.001), but it is not significant before 2019.
- **Physician share is positive but only marginal overall** (+0.10 pp, p < 0.10); it is significant after 2019 (+0.16 pp, p < 0.01).
- **Home health does not hold up.** The replication's negative home-health association is near zero and insignificant over 2014–2024.
- **FE logit agrees.** Higher inpatient and SNF shares both significantly lower the odds of achieving savings, and a higher physician share raises them (p < 0.05).

![Within-ACO change in spending shares since 2014](outputs/figures/spending_trends_2014_2024.png)

![Coefficient stability across samples](outputs/figures/coefficient_stability.png)

All extension models use ACO and performance-period fixed effects with ACO-clustered SEs. Full tables are in `outputs/extension_main_results.html`, `outputs/extension_robustness.html`, and `outputs/extension_spending_trends.html`; the knitted report is `outputs/ACO_Extension.html`.

---

## Differences from the Original Study

Full savings-rate model (Model 1 in both), ACO and year fixed effects:

| | Muhlestein et al. (2013–2016) | Replication (2014–2017) |
|---|---|---|
| Percent inpatient | −0.46* | −0.52** |
| Percent SNF | −0.82* | −0.29* |
| Percent physician | 0.23 | 0.05 |
| Percent home health | −0.31 | −0.34* |
| Percent DME | 1.16* | 0.18 |
| ACO-years (ACOs) | 1,377 (528) | 1,569 (614) |
| R² | 0.216 | 0.074 (within) |
| FE logit ACO-years (ACOs) | 624 (191) | 762 (236) |

\* p < 0.05, \*\* p < 0.01. The original reports robust SEs with \* p < .1, \*\* p < .05, \*\*\* p < .01; its stars are converted here.

- **Years differ.** The replication uses 2014–2017 rather than 2013–2016, since the 2013 file needs substantial cleaning.
- **Outliers.** The replication drops savings-rate outliers (1.5 × IQR); the original does not describe outlier handling.
- **Model fit.** The original does not say which R² it reports; the replication's is the within-ACO R².
- **Smaller logit samples.** A fixed-effects logit drops ACOs whose savings status never changes, which is why both logit samples are smaller than the savings-rate samples (45% of ACO-years in the original, 49% here).

---

## Repository Structure

```
analysis/
  ACO_Final.rmd       # full reproducible analysis (replication, 2014–2017)
  One_Pager.Rmd       # code for 1-page summary
  ACO_Extension.Rmd   # extension to 2014–2024

outputs/
  Medicare ACO Spending Patterns Replication Full Code Readout.pdf   # knitted full analysis
  One_Pager.pdf       # polished results summary
  combined_results.html, combined_results.docx,
  fe_models.html, logit_models.html                        # replication tables
  ACO_Extension.html  # knitted extension report
  extension_main_results.html, extension_robustness.html,
  extension_spending_trends.html                           # extension tables
  figures/            # extension figures

data/
  2014.csv – 2024.csv        # raw MSSP ACO Public Use Files (2019A.csv: Jul–Dec 2019)
  ssp-aco-id-crosswalk.xlsx  # CMS crosswalk from encrypted ACO_Num (2014–2017) to ACO_ID (2018+)
  aco_data.csv               # cleaned MSSP panel data, 2014–2017
  aco_data.rds               # cleaned MSSP panel data, 2014–2017 (R format)
  aco_panel_2014_2024.csv    # harmonized panel used by the extension

docs/
  ACO_Replication_Report.pdf   # original course paper (Dec 2024)
  Data_Dictionary-Medicare_Shared_Savings_Program-Performance_Year_Financial_and_Quality_Results_<year>.pdf
                               # CMS data dictionaries: 2013-2020, 2021, 2022, 2023, 2024
  Performance_Year_Financial_and_Quality_Results_PUF_Methodology.pdf   # CMS PUF methodology
```

---

## Reproduce the Analysis

1. Install R packages: tidyverse (includes readxl), janitor, plm, lmtest, sandwich, fixest, broom, modelsummary, flextable. `One_Pager.Rmd` knits to PDF and also needs LaTeX (for example, `tinytex::install_tinytex()`).
2. Knit `analysis/ACO_Final.rmd` in RStudio to rerun the replication; its tables are written to `outputs/`.
3. Knit `One_Pager.Rmd` to recreate the summary PDF.
4. Knit `ACO_Extension.Rmd` to rerun the 2014–2024 extension; tables and figures are written to `outputs/`.

File paths are relative to `analysis/`, so knit from RStudio or with `rmarkdown::render("analysis/<file>")`.

---

## Data Source

CMS, [Medicare Shared Savings Program Performance Year Financial and Quality Results](https://data.cms.gov/medicare-shared-savings-program/performance-year-financial-and-quality-results) public use files. Notes:

- 2019 has two files: `2019.csv` (full-year and January–June ACOs) and `2019A.csv` (ACOs that began new agreements on July 1, 2019).
- `2024.csv` is CMS's July 2026 revision.
- The 2014–2017 files identify ACOs with an encrypted `ACO_Num`; 2018+ use the CMS `ACO_ID`. CMS's [ACO ID crosswalk](https://www.cms.gov/medicare/payment/fee-for-service-providers/shared-savings-program-ssp-acos/data) (`data/ssp-aco-id-crosswalk.xlsx`) links the two; all 618 ACOs in the 2014–2017 files appear in it.
- The 2014–2017 files are the copies used for the original 2024 analysis. They match CMS's current files except that CMS now publishes 2014–2015 savings rates as percents rather than fractions, and these copies round 2016–2017 savings rates to four decimals (a difference of at most 0.005 pp).
- The CMS data dictionaries and PUF methodology note are in `docs/`. The savings rate and the per-beneficiary expenditure categories (`CapAnn_*`) have the same definitions in every year, with two exceptions: from 2022, ambulance services are identified with CMS's Restructured BETOS codes instead of BETOS codes, and from 2024, the savings rate for ACOs that began an agreement period on or after January 1, 2024 reflects CMS's guardrail policy.
- Ambulance spending (`CapAnn_AmbPay`) is a subset of physician/supplier spending (`CapAnn_PB`), so the eight-category total used as the denominator for spending shares counts it twice (about 1% of the total). Both the replication and the extension use this total.

---

## Original Study Citation

Muhlestein DB, Morrison SQ, Saunders RS, Bleser WK, McClellan MB, Winfield LD.  
*Medicare Accountable Care Spending Patterns: Shifting Expenditures Associated With Savings.*  
**American Journal of Accountable Care.** 2018;6(1):11–19.

---

## License

Code is released under the MIT License; see `LICENSE`. The data in `data/` and the CMS documents in `docs/` come from CMS (data.cms.gov and cms.gov).
