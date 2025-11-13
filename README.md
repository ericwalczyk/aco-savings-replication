# Medicare ACO Spending Patterns – Replication & Extension (2014–2017)

This project replicates and extends findings from **Muhlestein et al. (2018)**, *Medicare Accountable Care Spending Patterns: Shifting Expenditures Associated With Savings*, AJAC 6(1):11–19.  [oai_citation:0‡Medicare Accountable Care Spending Patterns.pdf](sediment://file_000000005c3471f598f95c85d9bdb816)

Using updated **2014–2017 MSSP ACO Public Use Files**, I recreate spending-share metrics and estimate fixed-effects models to test which expenditure shifts are associated with shared savings.

---

## Key Results (Short Version)

- **Lower inpatient and SNF spending shares** are consistently associated with higher savings.
- **Higher physician spending share** is modestly positive but less consistent.
- Results closely match the original study despite using later data.
- Findings hold under:
  - ACO & year fixed effects  
  - ACO‐clustered SEs  
  - Reduced models (multicollinearity)  
  - Fixed-effects logistic regression

**Bottom line:** ACOs that save money shift dollars **away from inpatient/SNF** toward **physician-directed outpatient care**.

---

## Repository Structure
analysis/
  ACO_Final.Rmd     # full reproducible analysis
  One_Pager.Rmd     # code for 1-page summary

output/
  One_Pager.pdf     # polished results summary

data/
  aco_data.rds      # cleaned MSSP panel data

  ---

## Reproduce the Analysis

1. Open `analysis/ACO_Final.Rmd` in RStudio.  
2. Install required packages (tidyverse, fixest, broom, flextable, etc.).  
3. Knit the file to regenerate all models and figures.  
4. Knit `One_Pager.Rmd` to recreate the summary PDF.

---

## Original Study Citation

Muhlestein DB, Morrison SQ, Saunders RS, Bleser WK, McClellan MB, Winfield LD.  
*Medicare Accountable Care Spending Patterns: Shifting Expenditures Associated With Savings.*  
**American Journal of Accountable Care.** 2018;6(1):11–19.  [oai_citation:1‡Medicare Accountable Care Spending Patterns.pdf](sediment://file_000000005c3471f598f95c85d9bdb816)

---

## License

MIT License. See `LICENSE` for details.
